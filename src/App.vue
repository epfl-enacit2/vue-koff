<template>
  <div id="app">
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
      people:[ ],
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
        let self = this
        person.payed += this.sumToPay()
        this.consume()
        this.resetSeleted()

        $.post( "http://localhost:3000/pay", { people: self.people });
      }
    },
    selectedPeople(){
      return this.people.filter((p)=> p.isSelected)
    },
    sumToPay(){
      return this.selectedPeople().length
    },
    consume(){
      this.selectedPeople().map(x => x.consumed++)
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

    $.getJSON( "http://localhost:3000/", function( data ) {
      self.people = data.people.map(u => {
        u.isSelected = false
        return u
      })
    });
  }
}
</script>

<style>
  body {
    font-family: Helvetica, sans-serif;
  }
  
  .abus {
    padding: 10px 5px 10px 5px;
    background-color: #D61A1A!important;
  }
  
  .reglo {
    padding: 10px 5px 10px 5px;
    background-color: #8DC718;
  }
  
  .selected {
    background-color: #65188F!important;
  }
</style>