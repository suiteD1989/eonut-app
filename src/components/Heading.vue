<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <h1>{{ this.introText }}</h1>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Heading',
    data() {
        return {
            introText: 'Earth Observatory Natural Events Tracker'
        }
    }
}
</script>

<style lang="scss" scoped>
    h1 {
        text-align: center;
        color: #177e89;
    }
</style>