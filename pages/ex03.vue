<template>
    <div>
      <h1>Hello {{ name }}</h1>
      <input type="text" :value="name">
    </div>
  </template>
<script>
   export default{
    data(){
    return {
      name: "chaiyaphum",
      age: 50,
    }
  }
   }
</script>