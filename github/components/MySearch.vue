<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="keyword"
      />&nbsp;<button @click="searchUser">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "MySearch",
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    searchUser() {
      this.$bus.$emit('getUsers',{
        isFirst:false,
        isLoading:true,
      })
      axios.get(`https://api.github.com/search/users?q=${this.keyword}`).then(
        response => {
          console.log('请求成功了');
          this.$bus.$emit('getUsers',{
            isLoading:false,
            users:response.data.items,
            errMsg:''
          })
        },
        error => {
          console.log('请求失败了');
          this.$bus.$emit('getUsers',{
            isLoading:false,
            users:[],
            errMsg:error.message
          })
        }
        )
    },
  },
  // https://api.github.com/search/users?q=xxxe
};
</script>

<style>
</style>