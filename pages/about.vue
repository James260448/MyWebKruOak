<template>
    <div class="profile">
      <div class="profile-image">
        <img :src="profileImage" alt="Profile Picture" />
      </div>
      <div class="profile-details">
        <h1>{{ name }}</h1>
        <p>{{ bio }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        profileImage: 'https://scontent.fbkk7-2.fna.fbcdn.net/v/t39.30808-1/380484964_1077755320301594_3661895525716141038_n.jpg?stp=dst-jpg_s200x200&_nc_cat=111&ccb=1-7&_nc_sid=0ecb9b&_nc_eui2=AeFUPgQF7dW-cFwCDKu1wMbCv4fKnYwTJye_h8qdjBMnJ8YGiF8BCxaltpyw_vs4KvfLERX1nYjQU6lLXLihV4mK&_nc_ohc=JarCX13l-RwQ7kNvgEnqP_5&_nc_ht=scontent.fbkk7-2.fna&oh=00_AYBgSRxPJDDjOhhBsdKA2HMvSVjOo-zbNPW4UKIUC8Mzgg&oe=66D37586',
        name: 'Thanathip Sethachutkul',
        bio: 'Our website provides an interface for connecting with MQTTX, allowing users to manage and monitor MQTT topics in real-time. Additionally, users can view and retrieve data stored in our database, ensuring seamless integration between MQTT communication and data management.',
      };
    },
  };
  </script>
  
  <style scoped>
  .profile {
    display: flex;
    align-items: center;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
    max-width: 600px;
    margin: 20px auto;
  }
  
  .profile-image img {
    border-radius: 50%;
    width: 150px;
    height: 150px;
    object-fit: cover;
    margin-right: 20px;
  }
  
  .profile-details h1 {
    margin: 0;
    font-size: 2em;
  }
  
  .profile-details p {
    margin: 10px 0 0;
    font-size: 1.2em;
  }
  </style>