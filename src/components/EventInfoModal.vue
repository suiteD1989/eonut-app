<template>
    <div class="row event-detail" :id="event.id">
        <div class="col-12 detail-data"> 
            <span>Event Title : </span>
            <span>{{ event.title }}</span>
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
        padding: 1em 0;
        visibility: hidden;
        opacity: 0;
        transition: visibility 0s, opacity 0.5s linear;
        background: #177e89;
        color: #fff;
        text-transform: capitalize;
        text-align: center;
        display: none;
        a {
            color: #fff;
            transition: all 0.25s linear;
            &:hover {
                color: #8e936d;
            }
        }
        .detail-data {
            padding: 0.5em 1em;
        }
    }
    .toggle-display {
        visibility: visible;
        opacity: 1;
        display: block;
    }
</style>