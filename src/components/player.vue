<template>
  <div>
    <h1>球员页面</h1>
    <p>{{detail}}</p>
  </div>
</template>

<script>
  export default {
    name: "player",
    data () {
      return {
        detail: {}
      }
    },
    mounted () {
      this.detail = this.getPlayer(this.$route.params.uid)
    },
    beforeRouteUpdate (to, from, next) {
      this.detail = this.getPlayer(to.params.uid)
      next()
    },
    methods: {
      getPlayer (uid) {
        switch (uid) {
          case '1':
            return { uid: 1, name: '库里', point: 26 };
          case '2':
            return { uid: 2, name: '哈登', point: 30 };
          default:
            return { uid: -1 }
        }
      }
    }
  }
</script>
