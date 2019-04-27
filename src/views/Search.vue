<template>
  <div class="wrapper">
    <div class="login">
      <label for="email">Email</label>
      <input type=text id="email" name="email" v-model="email"/>
      <label for="password">Password</label>
      <input type=password id="password" name="password" v-model="password"/>
      <input type=button @click="handleLogin" value="log-in" />
    </div>
    <div class="search">
      <label for="search">Search</label>
      <input
      id="search"
      name="search"
      v-model="searchValue"
      @input="handleInput"
      />
    </div>
    <ul >
      <li v-for="item in results" :key="item.id">
        <h3>{{ item.name }}</h3>
        <h4>{{ item.ip }}</h4> 
        <p>{{ item.description }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

const API = 'http://localhost:3000/'

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      email: '',
      password: '',
      auth_token: '',
      results: []
    };
  },
  methods: {
    handleInput() {
      if(this.auth_token !== '') {
        axios.get(`${API}devices`, { proxy: false, headers: { Authorization: this.auth_token } })
        .then((response) => {
          console.log(response);
          this.results = response.data
        })
        .catch((error) => {
          console.log(error)
        });
      }
    },
    handleLogin() {
      let self = this
      axios.post(`${API}authenticate`, {
        email: this.email,
        password: this.password
      })
      .then(function(response) {
        console.log(response)
        console.log(response.data.auth_token)
        self.auth_token = response.data.auth_token
      })
      .then(function(error) {
        console.log(error)
      })
    },
  },
};
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
    width: 100%;
  }

  .search {
    width: 300px;
    display: flex;
    flex-direction: column;

    label {
      font-family: Monsterrat, sans-serif;
    }

    input {
      height: 30px;
      border: 0;
      border-bottom: 1px solid gray;
    }
  }

  .login {
    display: flex;
    flex-direction: column;
    margin-bottom: 2em;

    label {
      font-size: 1.25em;
      padding: .2em;
      text-align: center;
    }

    input {
      padding: 1em;
      width: 250px;
      height: 50px
    }
  }
</style>
