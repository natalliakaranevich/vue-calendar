<template>
    <div class="calendar-wrap" >
        <div class="calendar" v-if="!createNewEvent">
            <div class="header">{{ year[currentMonth] && year[currentMonth].heading }}</div>
            <div class="main">
                <div class="navigation">
                    <a @click="this.showPrevMonth">prev</a>
                    <a @click="this.showNextMonth">next</a>
                </div>
                <div class="week"  v-for="week in year[currentMonth].data">
                    <div :class="{
                            today: isToday(day),
                            selected: isActiveDay(day),
                            gray: notInCurrentMonth(day),
                            weekends: day.wend,
                            day
                         }"
                         v-for="day in week"
                         @click="() => updateSelectedDate(day)">
                        {{day.day}}
                    </div>
                </div>
                <br>
                <div v-for="event in events" class="created-events">
                    {event.title}
                </div>
                <div class="actions">
                    <div @click="showToday"
                         :class="{show: showCurrentDateLink, 'link-to-current-day': true, action: true}">
                        {{ today }}
                    </div>
                    <div class="add-event action" @click="createEvent">+</div>
                </div>
            </div>
        </div>
        <div class="new-event" v-else>
            <div @click="closeCreateEventForm">&#10060;</div>
            <h1>Add new event to {{ year[currentMonth] && year[currentMonth].heading  }} - {{ today }}</h1>
            <form onsubmit="onSubmit">
                <custom-input label="Title" type="text" name="title" validate="required|email"/>
            </form>
        </div>
    </div>
</template>

<script lang="ts">
    // @ts-ignore
    import NBMomentCalendar from 'nb-moment-calendar';
    import moment from 'moment';
    import CustomInput from "@/components/formElements/custom-input.vue";

    const date = new Date();
    const currentYear = date.getFullYear();
    const nbMomentCalObj = new NBMomentCalendar;
    const year: object = nbMomentCalObj.getYear(currentYear, 'MMM YYYY');

    export default {
        name: 'Calendar',
        components: {CustomInput},
        data: () => ({
            year,
            currentYear,
            currentMonth: date.getMonth(),
            selectedDay: date.getDate(),
            today: date.getDate(),
            events: [],
            createNewEvent: false
        }),
        methods: {
            log: (data, name) => {
                console.log(name || '', JSON.parse(JSON.stringify(data)));
            },
            isToday: function(day) {
                const fday = moment(day.fday).format('MM-DD');
                const fToday =  moment(date).format('MM-DD');

                return fday === fToday;
            },
            isActiveDay: function(day) {
                const dayId = moment(day.fday).format('MM-DD');
                const selectedDayId = moment(`${this.currentMonth}-${+this.selectedDay}`).format('MM-DD');

                return  +day.day === +this.selectedDay;
            },
            notInCurrentMonth(day) {
                const currentMonth = moment(moment().months(this.currentMonth)).format('MM');
                const dayMonth = moment(day.fday).format('MM');

                return +dayMonth !== +currentMonth;
            },
            updateSelectedDate(day) {
                this.selectedDay = +day.day;
            },
            showPrevMonth() {
                if (this.currentMonth - 1 === -1) {
                    const newYear = this.currentYear - 1;
                    this.year = nbMomentCalObj.getYear(newYear, 'MMM YYYY');
                    this.currentYear = newYear;
                    this.currentMonth = 11;
                } else {
                    this.currentMonth = this.currentMonth - 1;
                }
            },
            showNextMonth() {
                if (this.currentMonth === 11) {
                    const newYear = this.currentYear + 1;
                    this.year = nbMomentCalObj.getYear(this.currentYear + 1, 'MMM YYYY');
                    this.currentYear = newYear;
                    this.currentMonth = 0;
                } else {
                    this.currentMonth = this.currentMonth + 1;
                }
            },
            showToday() {
                this.currentMonth = date.getMonth();
            },

            createEvent() {
                this.createNewEvent = true;
            },
            closeCreateEventForm() {
                this.createNewEvent = false;
            },
            onSubmit() {

            }
        },
        computed: {
            showCurrentDateLink() {
                return this.currentMonth !== date.getMonth();
            }
        },
        created(): void {

        }
    }
</script>

<style scoped lang="scss" src="../styles/styles.scss"/>
