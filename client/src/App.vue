<script>
import Cal from "./components/calender.vue";
import chart from "./components/chart.vue";
import split from "./components/spliter.vue";
import form2 from "./components/form.vue";
import mytable from "./components/mytable.vue";
import date from "./components/datepicker.vue"
export default {
  name: "App",
  methods: {
    clickEvent() {
      console.log("clicked");
    },
  },

  components: {
    Cal,
    chart,
    split,
    form2,
    mytable,
    date
  },

  data() {
    return {
      isDropdownOpen: false,
      currentComponent: "Cal",
      dropdownOptions: [
        {label:"Cal"},
        
        
        {label: "form2"},
        {label:"mytable"},
        {label:"date"},
        {label:"chart"},
      ],
      items: [
        { id: 1, name: "Apple" },
        { id: 2, name: "Banana" },
        { id: 3, name: "Orange" },
        { id: 4, name: "Mango" },
        { id: 5, name: "Grapes" },
        { id: 6, name: "Pineapple" },
      ],
      searchText: "",
      
    };
  },
  computed: {
    filteredItems() {
      if (!this.searchText) {
        return this.items;
      }

      const query = this.searchText.toLowerCase();
      return this.items.filter((item) =>
        item.name.toLowerCase().includes(query)
      );
    },
  },
  methods: {
    performSearch(searchQuery) {
      this.searchText = searchQuery;
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    selectOption(option) {
      this.currentComponent = option.label;
      this.isDropdownOpen = false;
    },
  },
};
</script>

<template>
  <div class="dropdown">
    <button @click="toggleDropdown" class="dropdown-toggle">
      {{ currentComponent }}
    </button>
    <ul v-if="isDropdownOpen" class="dropdown-menu">
      <li v-for="option in dropdownOptions" :key="option.id" @click="selectOption(option)">
        {{ option.label }}
      </li>
    </ul>
  </div>
   <component :is="currentComponent"></component>
</template>
<style>
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-toggle {
  padding: 10px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  cursor: pointer;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  margin-top: 5px;
  padding: 0;
  background-color: #fff;
  list-style: none;
  border: 1px solid #ccc;
}

.dropdown-menu li {
  padding: 10px;
  cursor: pointer;
}

.dropdown-menu li:hover {
  background-color: #f0f0f0;
}
</style>






