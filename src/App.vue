<template>
  <div id="app" >
    <div class="mdl-grid" v-if="screenSize > 400">
      <person v-for="person in sortedPeople" :guy="person" @pay="pay"></person>
    </div>
    <table v-else>
      <personrow v-for="person in sortedPeople" :guy="person" @pay="pay"></personrow>
    </table>
  </div>
</template>

<script>
import person from './components/person.vue'
import personrow from './components/personrow.vue'

export default {
  components: { person, personrow },
  data () {
    return {
      people:[
        {name: 'NiCrau', sciper: '262544', isSelected: false, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Vitor', sciper: '269711', isSelected: false, consumed: 0, payed: 0, hasPhoto: false},
        {name: 'Mickael', sciper: '240312', isSelected: false, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'André', sciper: '108332', isSelected: false, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Nicdub', sciper: '167916', isSelected: false, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Stefano', sciper: '150938', isSelected: false, consumed: 0, payed: 0, hasPhoto: true}
      ],
      koffPrice: 1.7,
      screenSize: window.innerWidth
    }
  },
  computed: {
    sortedPeople(){
      return this.people.sort((a, b) => {
        var sortStatus = 0;
        if ((a.consumed - a.payed) > (b.consumed - b.payed)) {
            sortStatus = -1;
        } else if ((a.consumed - a.payed) < (b.consumed - b.payed)) {
                sortStatus = 1;
        }
        return sortStatus;         
      })
    }
  },
  methods:{
    pay(person){
      if(person.isSelected){
        person.payed += this.sumToPay()
        this.consume()
        this.resetSeleted()
      }
    },
    selectedPeople(){
      return this.people.filter((p)=> p.isSelected)
    },
    sumToPay(){
      return this.selectedPeople().length * this.koffPrice
    },
    consume(){
      this.selectedPeople().map(x => x.consumed += this.koffPrice)
    },
    resetSeleted(){
      this.selectedPeople().map(x => x.isSelected = false)
    }
  },
  created () {
    let self = this
    window.onresize = () => {
      self.screenSize = window.innerWidth
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
.abus{
  padding: 10px 5px 10px 5px;
  background-color: #D61A1A!important;
}
.reglo{
  padding: 10px 5px 10px 5px;
  background-color: #8DC718;
}
.selected {
   background-color: #65188F!important;
}
</style>
