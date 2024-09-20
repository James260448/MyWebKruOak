<template>
    <div>
      <h1>Hello {{ name }}</h1>
      <div>{{ upperName }}</div>
      <input type="text" :value="name"><br>
      <input type="text" v-model="age"><br>
    </div>
  </template>
    <script>
       export default{
        data: () => ({
            name: 'chaiyaphum',
        }),
        computed:{
            upperName(){
             return this.name.toUpperCase()
            }
        }
       }
    </script>