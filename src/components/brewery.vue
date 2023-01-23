<template>
  <div :class="['brewery']">
    <h3>
      brewery id : {{ brewery.id }}
      <button @click="$emit('delete-brewery', [brewery.id,state.stateName])" class="delete"> delete </button>
    </h3>
    <p>state : {{ state.stateName }}</p>
    <p>city : {{ brewery.city }}</p>
    <p class="footer"> address : {{ brewery.street }} 
      <button value="update"  @click="update"> update </button>
    </p>
    <div v-show="showUpdate" class="update">
        <input type="text" class="input" v-model="newId" name="id"/>
        <input type="text" class="input" v-model="newState" name="state"/>
        <input type="text" class="input" v-model="newCity" name="city" />
        <input type="text" class="input" v-model="newStreet" name="street" />
        <input type="button" value="apply changes" @click="applyUpdate" class="apply-button"/> 
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyBrewery',
  props: {
    state: Object,
    brewery: Object,
  },
  data() {
    return {
      newId: this.brewery.id,
      newState: this.state.stateName,
      newCity: this.brewery.city,
      newStreet: this.brewery.street,
      showUpdate : false,
    }
  },
  emits: ['update-brewery','delete-brewery'],
  methods: {
      update(){
          this.showUpdate = !this.showUpdate
      },
      applyUpdate() {
        this.$emit('update-brewery', [this.state.stateName, 
          this.brewery.id, this.newId, this.newState, this.newCity, this.newStreet])       
      },
  },
}
</script>

<style scope>
.brewery{
  background: rgba(211, 211, 211, 0.8);
  padding: 20px;
  margin: 5px;
}
h3, .footer{
  display: flex;
  align-items: normal;
  justify-content: space-between;
}
.input{
    min-width: 80%;
    margin: 3px;
    align-items: center;
}
.update{
    display: flex;
    align-items: center;
    flex-direction: column;
}
.delete {
  color: red;
  cursor: normal;
}
</style>