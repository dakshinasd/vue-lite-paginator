<template>
  <div class="paginator" v-bind:class="classes" v-if="resource.length > 0">
      <div class="show-page-numbers" v-if="showPageNumbers">
          Page {{currentPage}} of {{totalPages}}
      </div>
      <button v-on:click="prev" class="pg-prev">Prev</button>
      <button v-on:click="next" class="pg-next">Next</button>
  </div>
</template>

<script>
export default {
    name: "paginator",
    data() {
        return {
            currentPage: 1,
            filteredResource: []
        }
    },
    props: {
        perPage: {
            type: Number,
            default: 3
        },
        resource: {
            type: Array,
            required: true
        },
        classes: {
            type: String
        },
        showPageNumbers: {
            type: Boolean,
            default: true
        }
    },
    methods : {
        filter() {
            if ( typeof(this.resource) != "undefined" && this.resource.length > 0 ) {
                
                this.filteredResource = this.resource.slice((this.currentPage * this.perPage) - this.perPage, (this.perPage * this.currentPage));

                this.$emit('updateResource', this.filteredResource);
            }
            else {
                // if there is an issue with the resource, emits this event to parent
                let errorPayload = {
                    message: "Resource is not defined"
                }

                this.$emit('resourceError', errorPayload);
            }
        },

        next() {
            if (this.currentPage < this.totalPages) {
                this.currentPage ++;
                this.filter()
            }
            else {
                return;
            }
        },

        prev() {
            if (this.currentPage > 1) {
                this.currentPage --;
                this.filter();
            }
            else {
                return;
            }
        }
    },
    mounted() {
       this.filter();
    },
    computed: {
        totalPages() {
            if (typeof this.resource != "undefined" && this.resource.length > 0 ) {
                return Math.ceil((this.resource.length / this.perPage));
            }
            else {
                return 1;
            }
        }
    },
    watch: {
        resource() {
            this.currentPage = 1;
            this.filter();
        }
    }
}
</script>


