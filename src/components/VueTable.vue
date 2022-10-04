<template>
  <div class="wrapper">
    <div class="filter">
      <select v-model="selected" @change="valueInput($event)" required>
        <option disabled value="">Выберете поле для фильтрации</option>
        <option v-for="title in titles" :key="title.id" :value="title.lable">
          {{ title.lable }}
        </option>
      </select>

      <select v-model="selectedCondition" @change="valueInput($event)" required>
        <option disabled value="">Выберите условие фильтрации</option>
        <option
          v-for="condition in conditions"
          :key="condition.id"
          :value="condition.id"
        >
          {{ condition.lable }}
        </option>
      </select>

      <input placeholder="Значение для фильтрации" v-model="value" />
    </div>

    <table>
      <tr class="td">
        <td>Дата</td>
        <td v-for="title in titles" :key="title.id" @click="sortData(title)">
          {{ title.lable }} <i> <img :src="changeImg" alt="" /></i>
        </td>
      </tr>

      <tbody>
        <tr v-for="row in paginated" :key="row.id">
          <td>{{ row.date }}</td>
          <td>{{ row.name }}</td>
          <td>{{ row.quantity }}</td>
          <td>{{ row.distance }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <div
        class="page"
        v-for="page in peges"
        :key="page"
        @click="onPage(page)"
        :class="{ page_selected: page === pageNumber }"
      >
        {{ page }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    rows: {
      type: Array,
      required: true,
    },
    titles: {
      type: Array,
      required: true,
    },
  },
  beforeMount() {
    this.rowsTable = this.rows;
  },

  data: () => ({
    usersPerPage: 4,
    pageNumber: 1,

    selected: "",
    selectedCondition: "",

    rowsTable: [],
    isAscending: true,
    value: "",
    conditions: [
      { id: 1, lable: "Равно" },
      { id: 2, lable: "Содержит" },
      { id: 3, lable: "Больше" },
      { id: 4, lable: "Меньше" },
    ],
  }),
  computed: {
    pageStar() {
      return (this.pageNumber - 1) * this.usersPerPage;
    },

    pageEnd() {
      return this.pageStar + this.usersPerPage;
    },

    changeImg() {
      return this.isAscending
        ? require("../assets/sort_ascending.svg")
        : require("../assets/sort_descending.svg");
    },

    filteredList() {
      console.log(this.selected);
      console.log(this.selectedCondition);

      if (
        this.selected == "" ||
        this.selectedCondition == "" ||
        this.value == ""
      ) {
        return this.rowsTable;
      }
      if (this.selectedCondition == 1) {
        return this.rowsTable.filter(
          (product) => product[this.selected.toLowerCase()] == this.value
        );
      }
      if (this.selectedCondition == 2) {
        return this.rowsTable.filter((product) =>
          product[this.selected.toLowerCase()]
            .toLowerCase()
            .includes(`${this.value}`)
        );
      }
      if (this.selectedCondition == 3) {
        return this.rowsTable.filter(
          (product) => product[this.selected.toLowerCase()] > this.value
        );
      }

      if (this.selectedCondition == 4) {
        return this.rowsTable.filter(
          (product) => product[this.selected.toLowerCase()] < this.value
        );
      }

      return this.rowsTable;
    },

    paginated() {
      return this.filteredList.slice(this.pageStar, this.pageEnd);
    },

    peges() {
      return Math.ceil(this.filteredList.length / this.usersPerPage);
    },
  },

  methods: {
    onPage(page) {
      this.pageNumber = page;
    },
    valueInput(e) {
      console.log(e.target.value);
    },
    sortData(title) {
      let target = title.lable.toLowerCase();

      if (this.isAscending) {
        this.rowsTable = this.rowsTable.sort((a, b) =>
          a[target] > b[target] ? 1 : -1
        );
      } else {
        this.rowsTable = this.rowsTable.sort((a, b) =>
          a[target] > b[target] ? -1 : 1
        );
      }

      this.isAscending = !this.isAscending;
    },
  },
};
</script>

<style>
.filter {
  display: flex;
  justify-content: flex-start;
  gap: 15px;
  margin-top: 20px;
  height: 40px;
  padding-bottom: 30px;
  width: 100%;
}

.filter select {
  height: 50px;
}

input {
  padding-left: 10px;
  height: 46px;
}

.wrapper {
  margin: 0 auto;
  width: 90%;
}
table {
  font-family: "Open Sans", sans-serif;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid #44475c;
}

table .td {
  text-transform: uppercase;
  text-align: left;
  background: #44475c;
  color: #fff;
  padding: 8px;
  min-width: 30px;
}

table .td th {
  display: flex;
}

table .td th i {
  justify-content: center;
  align-items: center;
  margin-left: 5px;
}

table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7d82a8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #d4d8f9;
}

.pagination {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  margin-top: 15px;
}

.page {
  padding: 8;
  border: 1px solid #d4d8f9;
  margin: 4px;
}

.page:hover {
  background-color: #585858;
  cursor: pointer;
  color: #fff;
}

.page_selected {
  background-color: #585858;
  cursor: pointer;
  color: #fff;
}
</style>
