<template>
  <div id="app">
    <h1>連絡先アプリ</h1>
    <div class="new">
      <h2>新規作成</h2>
      <div class="name">
        <label for="name">お名前：</label>
        <input type="text" name="name" id="name" v-model="newName">
      </div>
      <div class="email">
        <label for="email">メールアドレス</label>
        <input type="email" name="email" id="email" v-model="newEmail">
      </div>
      <button @click="insertcontacts">新規作成</button>
    </div>
    <div class="table">
      <h2>連絡リスト</h2>
      <table>
        <tr>
          <th>ID</th>
          <th>NAME</th>
          <th>EMAIL</th>
          <th>UPDATE</th>
          <th>DELETE</th>
        </tr>
        <tr v-for="item in contactLists" :key="item.id">
          <td>{{ item.id }}</td>
          <td><input type="text" v-model="item.name"></td>
          <td><input type="text" v-model="item.email"></td>
          <td>
            <button @click="updatecontacts(item.id, item.name, item.email)">
              更新
            </button>
          </td>
          <td>
            <button @click="deletecontacts(item.id)">削除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newName: "",
      newEmail: "",
      contactLists: [],
    };
  },
  methods: {
    async getcontacts() {
      const resData = await axios.get("http://127.0.0.1:8000/api/contact/");
      this.contactLists = resData.data.data;
    },
    async insertcontacts() {
      const sendData = {
        name: this.newName,
        email: this.newEmail,
      };
      await axios.post("http://127.0.0.1:8000/api/contact/", sendData);
      await this.getcontacts();
    },
    async updatecontacts(id, name, email) {
      const sendData = {
        name: name,
        email: email,
      };
      await axios.put("http://127.0.0.1:8000/api/contact" + id, sendData);
      await this.getcontacts();
    },
    async delete(id) {
      await axios.delete("http://127.0.0.1:8000/api/contact" + id);
      await this.getcontacts();
    },
  },
  created() {
    this.getcontacts();
  },
};
</script>

<style>
table,
td,
th {
  border: 1px solid #000;
  border-collapse: collapse;
  text-align: center;
}

td,
th {
  padding: 5px;
}

th {
  background: #f0e6cc;
}

</style>
