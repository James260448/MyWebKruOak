<template>
    <div>
      <h1>แสดงข้อมูลนักศึกษา</h1>
      <ul>
        <li v-for="st in status" :key="st.code">
          <span>{{ st.code }}</span>
          <span>{{ st.username }}</span>
          <span>{{ st.password }}</span>
        </li>
      </ul>
    </div>
  </template>
  <script>
   import axios from "axios"
    export default{
    data: () => ({
        students: [
            { id: '12345', username: 'James', password: '54321' },
            { id: '12345', username: 'James1', password: '54321' },
            { id: '12345', username: 'James2', password: '54321' },
            { id: '12345', username: 'James3', password: '54321' },
        ]
    }),
    async created(){
    const response = await axios.get('http://localhost:7000/list')
    const data = response.data
    this.status = response.data
    this.status = data.row
    console.log(data)
   }
}
</script>
  