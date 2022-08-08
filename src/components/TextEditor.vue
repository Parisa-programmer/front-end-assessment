<template>
  <div>

    <v-btn @click="click">click</v-btn>
  </div>
</template>
<script>

export default {
  name:'text-editor',
  data(){
    return {
      text: ''
    }
  },
  components: {

  },
  methods:{
    click(){
      alert(this.text)
    },

  }
}
</script>
