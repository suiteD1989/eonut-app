<template>
    <div class="container" v-if="sortedEvents.length > 0">
        <div class="row">
            <div v-for="heading in this.tableHeadings" 
                :key="heading"
                @click="sortEvents(heading.toLowerCase())"
                class="col event-heading">
                {{ heading }}
            </div>
        </div>
        
        <div v-for="event in sortedEvents" :key="event.id" class="row event" @click="toggleEvent(event.id)" v-bind:class="{ closed: event.closed }">
            <div class="col data-cell">
                {{ event.geometries[0].date }}
            </div>
            <div v-if="event.closed" class="col data-cell event-closed">
                Closed
            </div>
            <div  v-else class="col data-cell event-open">
                Open
            </div>
            <div class="col data-cell">
                {{ event.categories[0].title }}
            </div>
            <div class="col-12 data-row">
                <EventInfoModal :event="event"/>
            </div>
        </div>
    </div>
</template>

<script>

import EventInfoModal from './EventInfoModal';

export default {
    name: 'DataTable',
    components: {
        EventInfoModal
    },
    props: {
        nasaData: []
    },
    data() {
        return {
            tableHeadings: [
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
            
        },
        /**
        * @param { id } - ID associated with DOM element
        *  toggleEvent takes in the ID of an element in the DOM and toggles it's visibilty 
        */
        toggleEvent (id) {
            const event = document.getElementById(id);
            event.classList.toggle("toggle-display");
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
    .event-heading {
        border-top: solid #177e89 1px;
        border-bottom: solid #177e89 1px;
        padding: 1em 0;
        text-align: center;
        color: #177e89;
        font-size: 1.5em;
        transition: all 0.25s linear;
        &:hover {
            cursor: pointer;
            background: rgba(23, 126, 137);
            color: #fff;
        }
    }
    .event {
        border-bottom: solid #177e89 1px;
        transition: all 0.25s linear;
        &:hover {
            cursor: pointer;
            background: rgba(23, 126, 137, 0.4);
            border-bottom: solid white 1px;
            color: white;
        }
        .event-closed {
            color: #c31313;
        }
        .event-open {
            color: #169816;
        }
        .data-cell {
            text-align: center;
            font-size: 1.2em;
            padding: 1.5em 0;
        }
        .data-row {
            padding: 0;
        }
    }
    .closed {
        background: #d4d4d4;
    }
</style>