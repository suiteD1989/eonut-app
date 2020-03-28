<template>
    <div class="row toggle-display event-detail" :id="event.id">
        <div class="col-12 detail-data"> 
            {{ event.title }}
        </div>
        <div class="col-12 detail-data"> 
            <a :href="event.sources[0].url" target="_blank">Source</a>
        </div>
    </div>
</template>

<script>

export default {
    name: 'EventInfoModal',
    props: {
        event: {}
    }    
}

</script>

<style lang="scss" scoped>
    .event-detail {
        padding: 0;
        .detail-data {
            padding: 0;
        }
    }
    .toggle-display {
        display: none;
    }
</style>