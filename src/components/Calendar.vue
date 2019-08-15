<template>
    <div class="calendar-wrap">
        <div class="calendar">
            <div class="header">{{ year[currentMonth] && year[currentMonth].heading }}</div>
            <div class="main">
                <div class="actions">
                    <a @click="this.showPrevMonth">prev</a>
                    <a @click="this.showNextMonth">next</a>
                </div>
                <div class="week"  v-for="week in year[currentMonth].data">
                    <div :class="{
                            today: isToday(day),
                            selected: isActiveDay(day),
                            gray: notInCurrentMonth(day),
                            day
                         }"
                         v-for="day in week"
                         @click="() => updateSelectedDate(day)">
                        {{day.day}}
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script lang="ts">
    // @ts-ignore
    import NBMomentCalendar from 'nb-moment-calendar';
    import moment from 'moment';

    const date = new Date();
    const currentYear = date.getFullYear();
    const nbMomentCalObj = new NBMomentCalendar;
    const year: object = nbMomentCalObj.getYear(currentYear, 'MMM YYYY');

    export default {
        name: 'Calendar',
        data: () => ({
            year,
            currentYear,
            currentMonth: date.getMonth(),
            selectedDay: date.getDate(),
            today: date.getDate()
        }),
        methods: {
            log: (data, name) => {
                console.log(name || '', JSON.parse(JSON.stringify(data)));
            },
            isToday: function(day) {
                const fday = moment(day.fday).format('MM-DD');
                const fToday =  moment(date).format('MM-DD');

                if (+day.day === +this.today) {
                    debugger
                }
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
            }
        },
        computed: {
        },
        created(): void {

        }
    }
</script>

<style scoped lang="scss">
    $text-color: #64b5f6;

    .calendar-wrap {
        margin: auto;
        width: 80%;
        height: 600px;
        background: aliceblue;
        border-radius: 10px;
        display: flex;
        align-items: center;
        justify-content: center;

        .calendar {
            width: 90%;
            height: 90%;
            background: beige;

            .header {
                width: 100%;
                height: 50px;
                display: flex;
                align-items: center;
                justify-content: center;
                color: $text-color;
                font-weight: bold;
            }

            .main {
                padding: 0 20px;
            }
        }
    }

    .actions {
        display: grid;
        grid-template-columns: repeat(2, calc(100% / 2));
        margin-bottom: 30px;

        a {
            display: block;
            text-decoration: none;
            color: $text-color;
        }
    }

    .week {
        display: grid;
        grid-template-columns: repeat(7, calc(100% / 7));
        margin-bottom: 16px;

        .day {
            height: 40px;
            width: 40px;
            border-radius: 100%;
            background: #4db6ac;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #e0f2f1;

            &.selected {
                background: #004d40;
                opacity: 0.5;
            }

            &.today {
                background: #004d40;
                opacity: 1;
            }

            &.gray {
                opacity: 0.4;
            }
        }
    }
</style>
