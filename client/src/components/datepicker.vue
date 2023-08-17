<template>
    <div class="event-scheduler">
      <h2>Date picker</h2>
      <div class="event-form">
        <label for="event-date">Date picker:</label>
        <input type="date" id="event-date" v-model="eventDate" />
      </div>
      {{ eventDate }}
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        eventDate: "",
      };
    },
  };
  </script>
  
  <style>
  .event-form {
    margin-bottom: 20px;
  }
  
  .event-list {
    font-size: 16px;
  }
  
  .event-list ul {
    list-style-type: none;
    padding: 0;
  }
  
  .event-list li {
    margin-bottom: 5px;
  }
  
  .event-list button {
    margin-left: 10px;
  }
  </style>