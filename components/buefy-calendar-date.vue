<template>
    <div class="tile is-ancestor week">
        <div v-for="n in 7" class="tile card is-vertical is-parent day" :class="dates[n + offset].class">
            <p class="day-number" v-text="dates[n + offset].date"></p>
            <ul v-if="events">
                <li class="event" v-for="event in events[dates[n + offset].key]">
                    <a :href="event.url" target="_blank">
                        <span v-text="event.title"></span>
                    </a>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        props: ["dates", "events", "offset"]
    }
</script>

<style scoped>
    .week {
        height: 100px;
        display: flex;
    }
    .day-number {
        text-align: left;
        margin-left: 5px;
    }
    .day {
        padding: 0 !important;
        overflow: auto;
    }
    .event {
        text-align: left;
        font-size: 12px;
        background: cornflowerblue;
        color: white;
        margin: 2px;
        padding: 2px;
        border: 1px;
    }
    .event a {
        color: white;
    }
</style>