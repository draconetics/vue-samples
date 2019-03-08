<template>
  <v-app>
    <v-content>
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data () {
      return {
          items: [
              { title: 'Home', icon: 'dashboard', to:'/' },
              { title: 'About', icon: 'question_answer', to:'/about' }
          ],
          right: null
      }
  }
}
</script>
<style>

</style>
