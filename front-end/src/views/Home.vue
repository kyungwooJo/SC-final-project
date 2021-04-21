<template>
  <div>
    <HomePage v-if="user == null" />  
    <div v-else >
      <section id = "navbar">
        <div class = "nav-list">
            <ul class = "navbar-unlist">
                <li class = "nav-li current"><router-link to = "/">Home</router-link></li>
                <li class = "nav-li"> <router-link to = "/CategorizeScripture">Categorize</router-link></li>
                <li class = "nav-li"><router-link to = "/AddScripture">Add Scripture List</router-link></li>
            </ul>
        </div>  
      </section>
      <img id="mainLogo" src="../images/book-logo.png">   
      <h2 class="login-info">Logged in as: {{user.firstName}} {{user.lastName}} <button @click="logout" class="logout-btn pure-button pure-button-primary">Logout</button></h2>    
      <div id="totalListNum" class="marginPlus">
        <span>Total number of scriptures in list: </span><span class="spanRed">{{Object.keys(scriptures).length}}</span>
      </div>      
        <ScriptureList :scriptures="scriptures" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
import ScriptureList from "../components/ScriptureList.vue"
import HomePage from '@/components/HomePage.vue'
export default {
  name: 'Home',
  components: {
    HomePage,
    ScriptureList
  },

data() {
        return {
        scriptureList: [],
        scripture:null,
        book: "",
        topic: "",
       }
  }, 

    async created() {
    try {
      let response = await axios.get('/api/users');
      this.$root.$data.user = response.data.user;
    } catch (error) {
      this.$root.$data.user = null;
    }
  },

  methods: {
    expendScriptureList(){
      this.$root.$data.isExpend = true;
    },

    shirnkScriptureList(){
      this.$root.$data.isExpend = false;
    },

    async logout() {
      try {
        await axios.delete("/api/users");
        this.$root.$data.user = null;
      } catch (error) {
        this.$root.$data.user = null;
      }
    },

  },

  computed: {
    scriptures(){
      return this.$root.$data.myScriptureList;
    },
    user() {
      return this.$root.$data.user;
    }    
  }
}
</script>

<style>
  .marginPlus{
    margin-top:150px;
  }

  #btnWrap {
    display:flex;
    justify-content:center;
  }

  #btnWrap .expendBtn,
  .getBtn,
  .addBtn{
    padding:5px 5px;
    color: white;
    background: #4C8FFB;
    border: 1px #3079ED solid;
    box-shadow: inset 0 1px 0 #80B0FB;
    font-size:24px;
    margin-top:50px;
    margin-bottom:50px;
  }

  #btnWrap .shrinkBtn{
    padding:5px 5px;
    color: white;
    background: -webkit-linear-gradient(top, #DD4B39, #D14836); 
    border: 1px solid #DD4B39;
    box-shadow: inset 0 1px 0 #80B0FB;
    text-shadow: 0 1px 0 #C04131;
    font-size:24px;
    margin-top:50px;
    margin-bottom:50px;
  }

  .login-info{
    margin-top:30px;
    font-size:18px;
    text-align:center;
  }

  .logout-btn{
    border:none;
    color:purple;
    border-bottom:1px solid black;
  }
  
</style>

