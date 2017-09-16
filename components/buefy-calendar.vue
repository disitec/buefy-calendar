<template>
    <div class="container">
        <div class="columns is-hidden-mobile">
            <div class="column has-text-left"><a @click="previous()"><i class="fa button is-primary fa-chevron-left"></i></a></div>
            <div class="column is-half has-text-centered"><h2 class="title is-2" v-text="monthAndYear"></h2></div>
            <div class="column has-text-right"><a @click="next()"><i class="fa button is-primary fa-chevron-right"></i></a></div>
        </div>

        <div class="rows is-hidden-tablet">
            <div class="row has-text-centered"><a @click="previous()"><i class="button is-primary is-fullwidth fa fa-chevron-up"></i></a></div>
            <hr/>
            <div class="row has-text-centered"><h2 class="title is-2" v-text="monthAndYear"></h2></div>
            <hr/>
            <div class="row has-text-centered"><a @click="next()"><i class="button is-primary is-fullwidth fa fa-chevron-down"></i></a></div>
        </div>

        <div class="tile is-ancestor">
            <div v-for="weekday in weekdays" class="tile card is-vertical is-hidden-mobile">
                <header class="card-header">
                    <p class="card-header-title">
                        {{weekday}}
                    </p>
                </header>
            </div>
        </div>

        <calendar-date
                :key="n"
                v-for="n in 5"
                :events="appointments"
                :dates="dates"
                :offset="(n - 1) * 7"
        ></calendar-date>
    </div>
</template>

<script>
    import moment from 'moment';
    import CalendarDate from "./buefy-calendar-date.vue"

    const now = new moment(new Date().setDate(1));

    export default {
        props: {
          events: Object,
          weekdays: {
            type: Array,
            default: [
              "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"
            ],
          },
          months: {
            type: Array,
            default : [
              'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'
            ]
          },
          ordinals: {
            type: Boolean,
            default: true,
          },
          locale: {
            type: String,
            default: 'en',
          }
        },
        data() {
            return {
                appointments: this.events,
                showDates: true,
                currentMonth: now,
                dates: {},
            }
        },
        computed: {
          monthAndYear() {
            const month = this.currentMonth.format("M")
            const year = this.currentMonth.format("YYYY")
            return `${this.months[month - 1]} ${year}`;
          }
        },
        methods: {
          previous() {
            this.currentMonth = moment(this.currentMonth).subtract(1, "months");
            this.dates = this.getDates(this.currentMonth);
            this.$emit('previous');
          },
          next() {
            this.currentMonth = moment(this.currentMonth).add(1, "months");
            this.dates = this.getDates(this.currentMonth);
            this.$emit('next');
          },
          getDates(start) {
            const days = Array.apply(null, {length: moment().daysInMonth()})
              .map(Number.call, Number)
              .map((i) => {
                const d = start.clone().add(i, "days");
                return {date: this.ordinals ? d.format("Do") : d.format("D"), key: d.format("YYYY-MM-DD")};
              });

            var dates = new Array(start.day()).fill(this.getEmptyDate()).concat(days);
            var length = dates.length;
            dates.length = 36;
            dates.fill(this.getEmptyDate(), length);
            return dates;
          },
          getEmptyDate() {
            return {date: null, class: "is-hidden-mobile disabled"};
          },
        },
        components: {
            'calendar-date': CalendarDate
        },
        created() {
          this.dates = this.getDates(now);
        },
        watch: {
          events() {
            this.appointments = this.events;
          }
        },
    }
</script>