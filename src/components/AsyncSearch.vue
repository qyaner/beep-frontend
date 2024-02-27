<template>
    <div class="async">
        <div class="input-container">
            <input
                type="text"
                @input="onChange"
                v-model="search"
                @keydown.down="onArrowDown"
                @keydown.up="onArrowUp"
                @keydown.enter="onEnter"
            />
            <ul
                id="async-results"
                v-show="isOpen"
                ref="asyncResults"
                class="async-results"
            >
                <li class="loading" v-if="isLoading">
                Loading results...
                </li>
                <li
                v-else
                v-for="(result, i) in results"
                :key="i"
                @click="setResult(result)"
                class="async-result"
                ref="asyncResult"
                :class="{ 'is-active': i === arrowCounter }"
                >
                {{ result }}
                </li>
            </ul>
        </div>
    </div>
  </template>
  
  <script>
    export default {
      name: 'AsyncSearch',
      props: {
        items: {
            type: Array,
            required: false,
            default: () => [],
        },
        isAsync: {
            type: Boolean,
            required: false,
            default: false,
        },
      },
      data() {
        return {
            isOpen: false,
            results: [],
            search: '',
            isLoading: false,
            arrowCounter: -1,
        };
      },
      watch: {
        items: function (value, oldValue) {
            if (value.length !== oldValue.length) {
                this.results = value;
                this.isLoading = false;
            }
        },
      },
      mounted() {
        document.addEventListener('click', this.handleClickOutside)
        document.addEventListener('keydown', this.onKeyPress);
      },
      unmounted() {
        document.removeEventListener('click', this.handleClickOutside)
        document.removeEventListener('keydown', this.onKeyPress);
      },
      methods: {
        setResult(result) {
            this.search = result;
            this.isOpen = false;
        },
        filterResults() {
            const names = this.items.map(item => item.name);
            this.results = names.filter((name) => {
                return name.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
            });
        },
        onChange() {
            this.$emit('input', this.search);
    
            if (this.isAsync) {
                // Debounce the filterResults function
                const debouncedFilter = this.debounceSearch(this.filterResults, 300);
                debouncedFilter(); // Trigger the debounced function
                this.isLoading = true;
                setTimeout(() => {
                    this.filterResults();
                    this.isLoading = false; // Set isLoading to false after filtering
                    this.isOpen = true;
                }, 1000); // Simulated delay of 1 second
            } else {
                this.filterResults();
                this.isOpen = true;
            }
        },
        debounceSearch: function(func, delay) {
            let timeoutId;
            return function() {
                const context = this;
                const args = arguments;
                clearTimeout(timeoutId);
                timeoutId = setTimeout(function() {
                    func.apply(context, args);
                }, delay);
            };
        },
        handleClickOutside(event) {
            if (!this.$el.contains(event.target)) {
                this.isOpen = false;
                this.arrowCounter = -1;
            }
        },
        onKeyPress(event) {
            if (event.key === 'Escape') {
            this.onEscape();
            }
        },
        scrollResultIntoView() {
            const resultContainer = this.$refs.asyncResults;
            const resultItem = this.$refs.asyncResult[this.arrowCounter];
            if (resultContainer && resultItem) {
                const containerTop = resultContainer.scrollTop;
                const containerBottom = containerTop + resultContainer.clientHeight;
                const itemTop = resultItem.offsetTop;
                const itemBottom = itemTop + resultItem.offsetHeight;

                if (itemTop < containerTop) {
                    resultContainer.scrollTop = itemTop;
                } else if (itemBottom > containerBottom) {
                    resultContainer.scrollTop = itemBottom - resultContainer.clientHeight;
                }
            }
        },
        onArrowDown() {
            if (this.arrowCounter < this.results.length - 1) {
                this.arrowCounter = this.arrowCounter + 1;
                this.scrollResultIntoView();
            }
        },
        onArrowUp() {
            if (this.arrowCounter > 0) {
                this.arrowCounter = this.arrowCounter - 1;
                this.scrollResultIntoView();
            }
        },
        onEnter() {
            this.search = this.results[this.arrowCounter];
            this.isOpen = false;
            this.arrowCounter = -1;
        },
        onEscape() {
            this.isOpen = false;
            this.arrowCounter = -1;
        },
      },
    };
  </script>
  
  <style>
    .async {
        position: relative;
    }

    .input-container {
  position: relative;
  display: inline-block;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.async-results {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  padding: 0;
  margin: 0;
  border: 1px solid #ccc;
  width: 100%;
  max-height: 200px;
  overflow: auto;
  background-color: #fff;
  border-radius: 5px;
}

.async-result {
  list-style: none;
  text-align: left;
  padding: 10px;
  cursor: pointer;
}

.async-result.is-active,
.async-result:hover {
  background-color: #5f9ea0;
}

.loading {
  padding: 10px;
}
  </style>