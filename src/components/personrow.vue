<template>    
  <tr>
    <td @click="guy.isSelected = !guy.isSelected" :class="style">{{guy.name}}</td>
    <td @click="guy.isSelected = !guy.isSelected" :class="balance">{{ guy.consumed.toFixed(2)}} / {{ guy.payed.toFixed(2) }}</td>
    <td><a :disabled="!guy.isSelected" class="mdl-button mdl-button--colored mdl-js-button mdl-js-ripple-effect" @click="pay">Pay</a></td>
  </tr>
</template>

<script>
export default{
  props: ['guy'],
  computed:{
    style(){
      return this.guy.isSelected ? 'title-zone selected': 'title-zone'
    },
    photoUrl(){
      return `https://people.epfl.ch/cgi-bin/people/getPhoto?id=${this.guy.sciper}&show=`
    },
    balance(){
      return (this.guy.consumed > this.guy.payed) ? 'abus' : 'reglo'
    }
  },
  methods: {
    pay(){
      this.$emit('pay', this.guy)
    }
  }
}
</script>

<style>
.title-zone{
  color: #fff;
  text-shadow:
    1px 1px 0 #000,
    /* Simulated effect for Firefox and Opera
       and nice enhancement for WebKit */
   -1px -1px 0 #000,  
    1px -1px 0 #000,
   -1px  1px 0 #000,
    1px  1px 0 #000;
}
</style>
