<template>    
    <main>
      <addbrewery @add-brewery="addbrewery" />
      <list-of-broweries @delete-brewery="deletebrewery" @update-brewery="updateBrewery"
        :breweries="breweries"/>
    </main>
 </template>

<script>
  import addbrewery from './components/addbbrewery.vue'
  import listOfBroweries from './components/listOfBroweries.vue'
  
  
  export default {
    name: 'app',
    components: {
      listOfBroweries,
      addbrewery,
    },
    data() {
      return {
        mainObj: {},
        breweries: [],
      }
    },
    methods: {
      async fetchbreweries() { //fetching data using the api, and and making the mainObj.
          let mainOb = {states: {}}
          await fetch('https://api.openbrewerydb.org/breweries', {method: 'get'
              }).then(res => res.json()).then(data => {data.forEach(brewery =>  {
                const {state, id, city, street} = brewery
                let breweryy = {city: city, street: street}
                  if (mainOb.states[state]) {   //checking if state already exise
                      mainOb.states[state].breweries = {...mainOb.states[state].breweries,[id]: breweryy}                                                            
                  } 
                  else {
                    mainOb.states[state? state: "null"] = {stateName: state? state :"null", breweries: {[id]: breweryy}}                          
                  }
                })
              })
          this.mainObj = mainOb
          const mainObjArray = await Object.values(mainOb.states).sort((i,j) => i.stateName.localeCompare(j.stateName))//creating the array 
          this.breweries = await mainObjArray.map(s => {
            let tempBreArr = Object.entries(s.breweries).map(([i, j]) => {
              return {id: i, city: j.city, street: j.street}})
              const breweryArr = tempBreArr.sort((i,j) => i.city.localeCompare(j.city))
              return ({stateName: s.stateName, breweries: breweryArr})
          })
      },
      async addbrewery(brewery) { // add new brewery to the Library
        const[newState, newId, newCity, newStreet] = brewery
        if(this.mainObj.states[newState] && this.mainObj.states[newState].breweries[newId]) {
          alert("Duplication of breweries")
        }
        else {
          if (this.mainObj.states[newState]) {
            this.breweries = await this.breweries.map(s => {
              if (s.stateName == newState) { //checking if we need to orgnized the sort order
                return ({...s,breweries: [...s.breweries, 
                  {id: newId , city: newCity , street: newStreet }].sort((i,j) => i.city.localeCompare(j.city))})
              } 
              else {
                return s
              }
          })
            this.mainObj = {states: {...this.mainObj.states, [newState]: {...this.mainObj.states[newState],
                  breweries:{...this.mainObj.states[newState].breweries, [newId]: {city: newCity , street: newStreet}
                            }}}}
          } 
          else {
            this.breweries = [...this.breweries,{stateName: newState, breweries: [{id: newId , city: newCity , street: newStreet }]}]
            .sort((i,j) => i.stateName.localeCompare(j.stateName))
                  this.mainObj = {states: {...this.mainObj.states, [newState]: {stateName: newState,
                     breweries: {newId: { city: newCity, street: newStreet }}}}}
          }
        }
      },
      async deletebrewery(brewery) { //delete an exsiting brewery
         const [id,stateName] = brewery
         this.breweries = await this.breweries.map(s => {
           if (s.stateName == stateName) {
              return {
                ...s, breweries: s.breweries.filter(brewery => {return (brewery.id != id)})
              }
           }
           else {
             return s
           }
         })
         if (this.mainObj.states[stateName] && this.mainObj.states[stateName].breweries[id]) {
              let newStates = this.mainObj.states[stateName].breweries
              await delete newStates[id]
              this.mainObj = {states:{...this.mainObj.states,[stateName]: {...this.mainObj.states[stateName], breweries: newStates}}}    
         }
      },
      async updateBrewery(brewery) {//update exsiting brewery
      const [prevStateNmae, prevId, newId, newState, newCity, newAddress] = brewery
         await this.deletebrewery([prevId,prevStateNmae])
         await this.addbrewery([newState, newId, newCity, newAddress])
    },
    },
    async mounted() { 
       this.fetchbreweries()
    },
  }
  </script>
  
<style>
  
body{
    background-color:rgb(202, 144, 35);
    display: flex;
    background-image: url("https://upload.wikimedia.org/wikipedia/commons/7/76/577-beer-mug.svg");
} 

*{
  box-sizing: border-box;
  margin: 0;
  font-weight: normal;
}

h3, h4{
    color: brown;
}
h2,h3,h1{
    margin-bottom: 5px;
}

form{
    margin-top: 10px;
}
#app {
  display: flex;
  margin: 0 auto;
  font-weight: normal;
  }
  </style>
  