<template>
  <div class="calendar">
    <div class="header">
      <button @click="previousMonth">&lt;</button>
      <h2>{{ month }} {{ year }}</h2>
      <button @click="nextMonth">&gt;</button>
    </div>
    <table>
      <thead>
        <tr>
          <th v-for="day in daysOfWeek" :key="day">{{ day }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="week in calendar" :key="week">
          <td
            v-for="date in week"
            :key="date.date"
            :class="{
              'current-month': date.currentMonth,
              selected: date.selected,
            }"
            @click="selectDate(date)"
          >
            {{ date.date }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  {{ selected }}
</template>

<script>
export default {
  data() {
    return {
      currentDate: new Date(),
      selected: null,
      daysOfWeek: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
    };
  },
  computed: {
    month() {
      return this.currentDate.toLocaleString("default", { month: "long" });
    },
    year() {
      return this.currentDate.getFullYear();
    },
    calendar() {
      const year = this.currentDate.getFullYear();
      const month = this.currentDate.getMonth();
      const firstDayOfMonth = new Date(year, month, 1);
      const lastDayOfMonth = new Date(year, month + 1, 0);
      const firstDayOfWeek = firstDayOfMonth.getDay();

      const calendar = [];
      let week = [];

      for (let i = 0; i < firstDayOfWeek; i++) {
        week.push({
          date: "",
          currentMonth: false,
          selected: false,
        });
      }

      for (let i = 1; i <= lastDayOfMonth.getDate(); i++) {
        const date = new Date(year, month, i);
        const currentDate = new Date();

        week.push({
          date: i,
          currentMonth: true,
          selected: date.toDateString() === this.selectedDate?.toDateString(),
        });

        if (date.getDay() === 6 || i === lastDayOfMonth.getDate()) {
          calendar.push(week);
          week = [];
        }
      }

      return calendar;
    },
  },
  methods: {
    previousMonth() {
      this.currentDate = new Date(
        this.currentDate.getFullYear(),
        this.currentDate.getMonth() - 1,
        1
      );
    },
    nextMonth() {
      this.currentDate = new Date(
        this.currentDate.getFullYear(),
        this.currentDate.getMonth() + 1,
        1
      );
    },
    selectDate(date) {
      this.selected = new Date(
        this.currentDate.getFullYear(),
        this.currentDate.getMonth(),
        date.date
      );
    },
  },
};
</script>

<style>
.calendar {
  text-align: center;
}

.header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 0.5rem;
  border: 1px solid #ccc;
}

th {
  font-weight: bold;
}

td {
  cursor: pointer;
}

.current-month {
  background-color: #fff;
}

.selected {
  background-color: #ccc;
}
</style>
