<template>
  <div class="vl-paginator" v-bind:class="classes.container" v-if="resource.length > 0">
      <div class="vl-show-page-numbers" v-if="showPageNumbers" v-bind:class="classes.pageNoContainer">
          Page {{currentPage}} of {{totalPages}}
      </div>
      <button v-on:click="prev" class="vl-pg-prev" v-bind:class="classes.prev">Prev</button>
      <button v-on:click="next" class="vl-pg-next" v-bind:class="classes.next">Next</button>
  </div>
</template>

<script>
export default {
    name: "vl-paginator",
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
            type: Object,
            default() {
                return {
                    container: "", 
                    next: "", 
                    prev: "", 
                    pageo: ""
                }
            }
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
       console.log(this.classes)
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


