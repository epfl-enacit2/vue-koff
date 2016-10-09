<template>
  <div id="app"" >
    <div class="mdl-grid">
      <person v-for="person in sortedPeople" :guy="person" @pay="pay"></person>
    </div>
  </div>
</template>

<script>
import person from './components/person.vue'

export default {
  components: { person },
  data () {
    return {
      people:[
        {name: 'NiCrau', sciper: '262544', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Vitor', sciper: '269711', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: false},
        {name: 'Mickael', sciper: '240312', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'André', sciper: '108332', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Nicdub', sciper: '167916', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: true},
        {name: 'Stefano', sciper: '150938', isSelected: false, counter: 0, consumed: 0, payed: 0, hasPhoto: true}
      ],
      koffPrice: 1.7
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
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
</style>
