<template>    
  <div class="mdl-card mdl-shadow--2dp mdl-cell mdl-cell--3-col">
      <div :class="style" @click="guy.isSelected = !guy.isSelected">
        <div>
          <img v-if="guy.hasPhoto" :src="photoUrl" alt="">
          <img v-else src="../assets/dog.png" alt="">
        </div>
        <div>
          <h2 class="mdl-card__title-text">{{guy.name}}</h2>
        </div>
      </div>
      <div :class="balance">
        <div>Drinked/Payed: {{ guy.consumed}} / {{ guy.payed }}</div>
      </div>
      <div class="mdl-card__actions mdl-card--border">
        <a :disabled="!guy.isSelected" class="mdl-button mdl-button--colored mdl-js-button mdl-js-ripple-effect" @click="pay">Pay</a>
      </div>
  </div>
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
.demo-card-square.mdl-card {
  width: 320px;
  height: 320px;
}
.demo-card-square > .mdl-card__title {
  color: #fff;
  background: bottom right 15% no-repeat #46B6AC;
}

img {
    overflow: hidden;
    height: 150px;
    padding: 20px 20px 20px 20px;
}

.title-zone{
  min-height: 218.667px;
}

.title-zone *{
  color: #fff;
  margin: 0;
  position: absolute;
  top: 35%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%) 
}

.mdl-card__title-text{
  color: #fff;
  text-shadow:
    1px 1px 0 #000,
    /* Simulated effect for Firefox and Opera
       and nice enhancement for WebKit */
   -1px -1px 0 #000,  
    1px -1px 0 #000,
   -1px  1px 0 #000,
    1px  1px 0 #000;
  transform: translate(-50%, 150%);
}
</style>