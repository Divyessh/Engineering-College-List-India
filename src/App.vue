<template>
  <div id="app">
    <input
      type="text"
      placeholder="Filter by department or employee"
      v-model="filter"
    />

    <table>
      <thead>
        <tr>
          <th>College Name</th>
          <th>Address</th>
          <th>Email</th>
          <th>Phone</th>
          <th>Website</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in filteredRows" :key="`colleges-${index}`">
          <td v-html="highlightMatches(row.college_name)"></td>
          <td v-html="highlightMatches(row.address)"></td>
          <td v-html="row.email"></td>
          <td v-html="row.phone"></td>
          <td v-html="row.website"></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import json_data from "../public/data.json";
export default {
  name: "App",
  data() {
    return {
      filter: "",
      rows: json_data,
    };
  },
  methods: {
    highlightMatches(text) {
      const matchExists = text
        .toLowerCase()
        .includes(this.filter.toLowerCase());
      if (!matchExists) return text;

      const re = new RegExp(this.filter, "ig");
      return text.replace(
        re,
        (matchedText) => `<strong>${matchedText}</strong>`
      );
    },
  },
  computed: {
    filteredRows() {
      return this.rows.filter((row) => {
        const colleges = row.college_name.toString().toLowerCase();
        const addresses = row.address.toString().toLowerCase();
        const searchTerm = this.filter.toString().toLowerCase();

        return colleges.includes(searchTerm) || addresses.includes(searchTerm);
      });
    },
  },
};
</script>

<style>
table {
  font-family: arial, sans-serif;
  table-layout: fixed;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

th {
  background-color: #dddddd;
}

input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 25px;
}
</style>
