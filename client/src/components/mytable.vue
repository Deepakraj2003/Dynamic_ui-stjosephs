<template>
  <div>
    <table>
      <thead>
        <tr>
          <th v-for="column in tableColumns" :key="column.key" @click="sort(column.key)">
            {{ column.label }}
            <span v-if="sortKey === column.key">
              {{ sortDir === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in paginatedItems" :key="item._id">
          <td v-for="column in tableColumns" :key="column.key">
            {{ item[column.key] }}
          </td>
        </tr>
      </tbody>
    </table>
    <div>
      <input type="text" v-model="filterText" placeholder="Filter" />
      <button @click="resetFilter">Reset</button>
    </div>
    <div>
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';

export default {
  props: {
    perPage: {
      type: Number,
      default: 10,
    },
  },
  data() {
    return {
      currentPage: 1,
      filterText: '',
      sortKey: '',
      sortDir: 'asc',
      tableData: [],
      tableColumns: [],
    };
  },
  computed: {
    filteredItems() {
      if (!this.filterText) {
        return this.tableData;
      }

      const filterText = this.filterText.toLowerCase();
      return this.tableData.filter((item) => {
        return Object.values(item).some((value) => {
          return String(value).toLowerCase().includes(filterText);
        });
      });
    },
    sortedItems() {
      if (!this.sortKey) {
        return this.filteredItems;
      }

      const sorted = this.filteredItems.sort((a, b) => {
        const valueA = a[this.sortKey];
        const valueB = b[this.sortKey];

        if (typeof valueA === 'string' && typeof valueB === 'string') {
          return valueA.localeCompare(valueB);
        } else {
          return valueA - valueB;
        }
      });

      return this.sortDir === 'asc' ? sorted : sorted.reverse();
    },
    paginatedItems() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = start + this.perPage;

      return this.sortedItems.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.sortedItems.length / this.perPage);
    },
  },
  methods: {
    sort(key) {
      if (this.sortKey === key) {
        this.sortDir = this.sortDir === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortKey = key;
        this.sortDir = 'asc';
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    resetFilter() {
      this.filterText = '';
    },
    fetchData() {
      axios.get('http://localhost:3000/tabledata')
        .then((response) => {
          this.tableData = response.data;
        })
        .catch((error) => {
          console.error(error);
        });

      axios.get('http://localhost:3000/tablecolumns')
        .then((response) => {
          this.tableColumns = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>
