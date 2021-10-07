<template>
    <div class="contect">
        <h2>カレンダー{{ displayDate }}</h2>
        <div class="button-area">
            <button @click="prevMonth">前の月</button>
            <button @click="nextMonth">次の月</button>
        </div>
        <div class="calendar">
            <div class="calendar-weekly">
                <div class="calendar-youbi" v-for="n in 7" :key="n">
                    {{ youbi(n - 1) }}
                </div>
            </div>
            <div
                class="calendar-weekly"
                v-for="(week, index) in calendars"
                :key="index"
            >
                <div
                    class="calendar-daily"
                    :class="{ outside: currentMonth !== day.month }"
                    v-for="(day, index) in week"
                    :key="index"
                >
                    <div class="calendar-day">{{ day.date }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import moment from "moment";

export default {
    data() {
        return {
            currentDate: moment(),
            events: [
                {
                    id: 1,
                    name: "ミーティング",
                    start: "2021-10-01",
                    end: "2021-10-01",
                    color: "blue",
                },
                {
                    id: 2,
                    name: "イベント",
                    start: "2021-10-02",
                    end: "2021-10-03",
                    color: "limegreen",
                },
                {
                    id: 3,
                    name: "会議",
                    start: "2021-10-06",
                    end: "2021-10-06",
                    color: "deepskyblue",
                },
                {
                    id: 4,
                    name: "有給",
                    start: "2021-10-08",
                    end: "2021-10-08",
                    color: "dimgray",
                },
                {
                    id: 5,
                    name: "海外旅行",
                    start: "2021-10-08",
                    end: "2021-10-11",
                    color: "navy",
                },
                {
                    id: 6,
                    name: "誕生日",
                    start: "2021-10-16",
                    end: "2021-10-16",
                    color: "orange",
                },
                {
                    id: 7,
                    name: "イベント",
                    start: "2021-10-12",
                    end: "2021-10-15",
                    color: "limegreen",
                },
                {
                    id: 8,
                    name: "出張",
                    start: "2021-10-12",
                    end: "2021-10-13",
                    color: "teal",
                },
                {
                    id: 9,
                    name: "客先訪問",
                    start: "2021-10-14",
                    end: "2021-10-14",
                    color: "red",
                },
                {
                    id: 10,
                    name: "パーティ",
                    start: "2021-10-15",
                    end: "2021-10-15",
                    color: "royalblue",
                },
                {
                    id: 12,
                    name: "ミーティング",
                    start: "2021-10-18",
                    end: "2021-10-19",
                    color: "blue",
                },
                {
                    id: 13,
                    name: "イベント",
                    start: "2021-10-21",
                    end: "2021-10-21",
                    color: "limegreen",
                },
                {
                    id: 14,
                    name: "有給",
                    start: "2021-10-20",
                    end: "2021-10-20",
                    color: "dimgray",
                },
                {
                    id: 15,
                    name: "イベント",
                    start: "2021-10-25",
                    end: "2021-10-28",
                    color: "limegreen",
                },
                {
                    id: 16,
                    name: "会議",
                    start: "2021-10-21",
                    end: "2021-10-21",
                    color: "deepskyblue",
                },
                {
                    id: 17,
                    name: "旅行",
                    start: "2021-10-23",
                    end: "2021-10-24",
                    color: "navy",
                },
                {
                    id: 18,
                    name: "ミーティング",
                    start: "2021-10-28",
                    end: "2021-10-28",
                    color: "blue",
                },
                {
                    id: 19,
                    name: "会議",
                    start: "2021-10-12",
                    end: "2021-10-12",
                    color: "deepskyblue",
                },
                {
                    id: 20,
                    name: "誕生日",
                    start: "2021-10-30",
                    end: "2021-10-30",
                    color: "orange",
                },
            ],
        };
    },
    methods: {
        getStartDate() {
            let date = moment(this.currentDate);
            date.startOf("month");

            const youbiNum = date.day();

            return date.subtract(youbiNum, "days");
        },
        getEndDate() {
            let date = moment(this.currentDate);
            date.endOf("month");

            const youbiNum = date.day();

            return date.add(6 - youbiNum, "days");
        },
        getCalender() {
            let startDate = this.getStartDate();
            const endDate = this.getEndDate();
            const weekNumber = Math.ceil(endDate.diff(startDate, "days") / 7);

            let calendars = [];
            let calendarDate = this.getStartDate();

            for (let week = 0; week < weekNumber; week++) {
                let weekRow = [];
                for (let day = 0; day < 7; day++) {
                    let dayEvents = this.getDayEvents(calendarDate);
                    weekRow.push({
                        day: calendarDate.get("date"),
                        month: calendarDate.format("YYYY-MM"),
                        dayEvents,
                    });
                    calendarDate.add(1, "days");
                }
                calendars.push(weekRow);
            }

            return calendars;
        },
        nextMonth() {
            this.currentDate = moment(this.currentDate).add(1, "month");
        },
        prevMonth() {
            this.currentDate = moment(this.currentDate).subtract(1, "month");
        },
        youbi(dayIndex) {
            const week = ["日", "月", "火", "水", "木", "金", "土"];
            return week[dayIndex];
        },
    },
    computed: {
        calendars() {
            return this.getCalender();
        },
        currentMonth() {
            return this.currentDate.format("YYYY-MM");
        },
        displayDate() {
            return this.currentDate.format("YYYY[年]M[月]");
        },
    },
    mounted() {
        console.log(this.getCalender());
    },
};
</script>

<style>
.content {
    margin: 2em auto;
    width: 900px;
}
.button-area {
    margin: 0.5em 0;
}
.button {
    padding: 4px 8px;
    margin-right: 8px;
}
.calendar {
    max-width: 900px;
    border-top: 1px solid #e0e0e0;
    font-size: 0.8em;
}
.calendar-weekly {
    display: flex;
    border-left: 1px solid #e0e0e0;
    /* background-color: black; */
}
.calendar-daily {
    flex: 1;
    min-height: 125px;
    border-right: 1px solid #e0e0e0;
    border-bottom: 1px solid #e0e0e0;
    margin-right: -1px;
}
.calendar-day {
    text-align: center;
}
.calendar-youbi {
    flex: 1;
    border-right: 1px solid #e0e0e0;
    margin-right: -1px;
    text-align: center;
}
.outside {
    background-color: #f7f7f7;
}
</style>
