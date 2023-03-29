<template>
  <div>
    <BuscaRepo
      @ShowView="Show_View"
      @getUser="get_User"
      v-if="viewShow === true"
    />
  </div>
</template>

<script>

  import BuscaRepo from './components/BuscaRepo.vue'

  window.addEventListener("load", (e) => {
    e.preventDefault();
    localStorage.clear();
  })

  export default {
    name: 'App',
    components: {
      BuscaRepo
    },
    data() {
      const savedView = localStorage.getItem('view')
      const savedUser = localStorage.getItem('user')
      return {
        viewShow: savedView ? savedView : true,
        user: savedUser ? savedUser : ''
      }
    },
    methods: {
      Show_View() {
        this.viewShow = !this.viewShow
      },
      get_User() {
        const text = document.getElementById('input-group')
        if (this.user === '') {
          this.user = text.value
        } else {
          this.user = ''
        }
      }
    },
    watch: {
      viewShow(val) {
        localStorage.setItem('view', val)
      },
      user(val) {
        localStorage.setItem('user', val)
      },
      mounted() {
        alert(this.user)
      }
    }
  }
</script>


<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  text-decoration: none;
  list-style-type: none;
  font-family: 'Open Sans', sans-serif;
}
</style>
