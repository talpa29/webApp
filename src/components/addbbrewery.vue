<template>
  <form @submit="submit" class="adder">
    <h1>Brewery list</h1>
    <div class="form-control">
      <input type="text" v-model="id" name="id" placeholder="id" />
      <input type="text" v-model="state" name="state" placeholder="state" />
      <input type="text" v-model="city" name="city" placeholder="city" />
      <input type="text" v-model="street" name="street" placeholder="address"/>
      <p><input type="submit" value="Add brewery" class="btn btn-block" /></p>
    </div>
  </form>
</template>

<script>
export default {
  name: 'AddBrewery',
  data() {
    return {
      id: '',
      state: '',
      city: '',
      street: '',
    }
  },
  methods: {
    submit(event) {
      event.preventDefault()
      const newBrewery = [
        this.state,
        this.id,
        this.city,
        this.street,
      ]
      this.$emit('add-brewery', newBrewery)
      this.state = ''
      this.id = ''
      this.street = ''
      this.city = ''
    },
  },
}
</script>

<style scoped>
.adder {
  padding-bottom: 20px;
}
h1{
  text-align: center;
}
.form-control input {
  width: 99%;
  padding: 5px;
  margin: 5px;
  font-size: 18px;
}
</style>