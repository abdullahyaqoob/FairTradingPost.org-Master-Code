<template>
  <div>
    <h2>Fairtrader Mediator Admin Pannel</h2>
    <table class="styled-table">
      <thead>
        <tr>
          <!-- <th>User Id</th> -->
          <th>User Image</th>
          <th>User Name</th>
          <th>User Email</th>
          <th>User Country</th>
          <th>User Wallet</th>
          <th>User Facebook</th>
          <th>Created At</th>
        </tr>
      </thead>
      <tbody>
        <tr class="table-row" v-for="user in userData.reverse()" :key="user">
        <!-- <tr class="table-row" v-for="user in userData" :key="user"> -->
          <!-- <td>{{ user.id }}</td> -->
          <td><img :src="user.idCard" width="40px" onerror="this.src='https://static.thenounproject.com/png/17241-200.png'" /></td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.country }}</td>
          <td>{{ user.cardNumber }}</td>
          <a :href="user.facebookLink" style="color:#4C4646"><td>{{ user.facebookLink }}</td></a>
          <td>{{ user.createdAt }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      userToken: "",
      testUserID: "",
      userData: [],
    };
  },
  mounted() {
    window.scrollTo(0, 0);

    this.userToken = localStorage.getItem("UserToken");
    console.log("uesrToken", this.userToken);
    this.testUserID = localStorage.getItem("userID");
    console.log("User ID: ", this.testUserID);
    const token = this.userToken;
    axios
      .get(process.env.VUE_APP_SERVICE_URL + "User", {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      })
      .then((res) => {
        this.userData = res.data.users;
        console.log("UserData", this.userData);
      })
      .catch((err) => {
        this.$toasted.error("Something went wrong, Refresh the Page.");
        console.log("error", err);
      });
  },
};
</script>

<style scoped>
h2 {
  font-size: 40px;
  margin: 20px 0;
  text-align: center;
}
.styled-table {
  border-collapse: collapse;
  margin: 25px 0;
  font-size: 0.9em;
  font-family: sans-serif;
  /* min-width: 400px; */
  width: 90%;
  margin: 0 auto;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.styled-table thead tr {
  background-color: #009879;
  color: #ffffff;
  text-align: left;
}
.styled-table th,
.styled-table td {
  padding: 12px 15px;
}
.styled-table tbody tr {
  border-bottom: 1px solid #dddddd;
}

.styled-table tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}

.styled-table tbody tr:last-of-type {
  border-bottom: 2px solid #009879;
}
.styled-table tbody tr.active-row {
  font-weight: bold;
  color: #009879;
}
</style>