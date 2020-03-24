<template>
    <div>
        <table class="table is-fullwidth">
            <thead>
                <tr>
                    <th v-for="heading in this.tableHeadings" 
                        :key="heading"
                        @click="sortEvents(heading.toLowerCase())">
                        {{ heading }}
                    </th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="event in sortedEvents" :key="event.EONET_4618">
                    <td>
                        {{ event.title }}
                    </td>
                    <td>
                        {{ event.geometries[0].date }}
                    </td>
                    <td v-if="event.closed">
                        closed
                    </td>
                    <td v-else>
                        open
                    </td>
                    <td>
                        {{ event.categories[0].title }}
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'DataTable',
    props: {
        nasaData: []
    },
    data() {
        return {
            tableHeadings: [
                'Title',
                'Date',
                'Status',
                'Category'
            ],
            events: [],
            initSort: 'title',
            initSortDirection: 'asc'
        }
    },
    methods: {
        /**
        * @param { selected } - The table heading used to filter the displayed results
        * sortEvents takes the selected input & asigns it to the data prop initSort
        */
        sortEvents(selected) {
            if(selected === this.initSort) {
                this.initSortDirection = this.initSortDirection === 'asc' ? 'desc' : 'asc';
            }
            this.initSort = selected;
        },
        /**
        * @param { key } - This string is used to access object props
        * @param { event } - This is the event object
        * returnObjVal takes a key and an object and return the value of the object property 
        */
        returnObjVal(key, event) {
            let objValue = '';
            
            if (key === 'title') {
                return event.title;
            } else if (key === 'date' && event) {
                objValue = event.geometries[0].date;
                return objValue;
            } else if (key === 'status') {
                if (!event.closed) {
                    return 'open';
                } else {
                    return 'closed';
                }
            } else if (key === 'category' && event) {
                objValue = event.categories[0].title;
                return objValue;
            }
            
        }
    },
    computed:{
        sortedEvents() {
            return this.nasaData.slice().sort((a,b) => {
                let modifier = 1;
                if (this.initSortDirection === 'desc') {
                    modifier = -1;
                }
                if (this.returnObjVal(this.initSort, a) < this.returnObjVal(this.initSort, b)) {
                    return -1 * modifier;
                }
                if (this.returnObjVal(this.initSort, a) > this.returnObjVal(this.initSort, b)) {
                    return 1 * modifier;
                }
                return 0;
            });
        }
    }
}
</script>

<style lang="scss" scoped>

</style>