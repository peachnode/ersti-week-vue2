<script>
export default {
  emits: ['create-friend'],
  methods: {
    createFriend() {
      this.$emit('create-friend', this.phone, this.name, this.email);
      this.phone = "";
      this.name = "";
      this.email = "";
    }
  },
  data() {
    return {
      phone: "",
      name: "",
      email: ""

    };
  }
}

</script>

<template>
  <li>
    <div>
      <label style="padding: 10px">Name</label>
      <input v-model="name"> <br>
    </div>
    <div>
      <label style="padding: 8px">Phone</label>
      <input v-model="phone"> <br>
    </div>
    <div>
      <label style="padding: 10px">Email</label>
      <input v-model="email"> <br>
    </div>
    <div>
      <button @click="createFriend">Create friend</button>
    </div>

  </li>
</template>

<style scoped>

</style>