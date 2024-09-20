<template>
  <v-container fluid style="background-image: url('https://images.pexels.com/photos/125457/pexels-photo-125457.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1'); background-size: cover; background-position: center;">
    <v-row class="my-5">
      <!-- Temperature and Humidity Card on the left -->
      <v-col cols="12" md="6">
        <v-card class="pa-5" style="border-radius: 15px;" hover outlined>
          <v-card-title class="text-center">
            <h3>อุณหภูมิและความชื้น</h3>
          </v-card-title>
          <v-card-text>
            <v-row>
              <!-- อุณหภูมิ -->
              <v-col cols="12" sm="6">
                <v-card style="border-radius: 15px;" class="pa-3" hover outlined>
                  <v-card-title class="text-center">อุณหภูมิ</v-card-title>
                  <v-card-text class="text-center">
                    <v-progress-linear :value="value_temp" height="25" color="red">
                      <template v-slot:default>
                        {{ value_temp }} °C
                      </template>
                    </v-progress-linear>
                  </v-card-text>
                </v-card>
              </v-col>

              <!-- ความชื้น -->
              <v-col cols="12" sm="6">
                <v-card style="border-radius: 15px;" class="pa-3" hover outlined>
                  <v-card-title class="text-center">ความชื้น</v-card-title>
                  <v-card-text class="text-center">
                    <v-progress-linear :value="value_humi" height="25" color="blue">
                      <template v-slot:default>
                        {{ value_humi }} %
                      </template>
                    </v-progress-linear>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>

            <v-row class="mt-5">
              <!-- อุณหภูมิที่เหมาะสม -->
              <v-col cols="12" sm="6">
                <v-card style="border-radius: 15px;" class="pa-3" hover outlined>
                  <v-card-title class="text-center">อุณหภูมิที่เหมาะสม</v-card-title>
                  <v-card-text class="text-center">
                    <v-progress-circular :model-value="totalValueT()" size="150" width="20" color="amber">
                      {{ totalValueT() }} °C
                    </v-progress-circular>
                    <v-row class="mt-3">
                      <v-col cols="6" class="text-center">
                        <div>ต่ำสุด</div>
                        <div>{{ value_t1 }} °C</div>
                      </v-col>
                      <v-col cols="6" class="text-center">
                        <div>ไม่เกิน</div>
                        <div>{{ value_t2 }} °C</div>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-card>
              </v-col>

              <!-- ความชื้นที่เหมาะสม -->
              <v-col id="button-section" cols="12" sm="6">
                <v-card style="border-radius: 15px;" class="pa-3" hover outlined>
                  <v-card-title class="text-center">ความชื้นที่เหมาะสม</v-card-title>
                  <v-card-text class="text-center">
                    <v-progress-circular :model-value="totalValueH()" size="150" width="20" color="green">
                      {{ totalValueH() }} %
                    </v-progress-circular>
                    <v-row class="mt-3">
                      <v-col cols="6" class="text-center">
                        <div>ต่ำสุด</div>
                        <div>{{ value_h1 }} %</div>
                      </v-col>
                      <v-col cols="6" class="text-center">
                        <div>ไม่เกิน</div>
                        <div>{{ value_h2 }} %</div>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Control Card on the right -->
      <v-col cols="12" md="6">
        <v-card style="border-radius: 15px;" class="pa-5" hover outlined>
          <v-row justify="space-around" class="mb-5">
            <v-col cols="12" sm="5">
              <v-btn block color="green" class="white--text" large @click="start" style="border-radius: 15px;height: 50px;">
                เริ่มต้น
              </v-btn>
            </v-col>
            <v-col cols="12" sm="5">
              <v-btn block color="red" class="white--text" large @click="stop" style="border-radius: 15px;height: 50px;">
                หยุด
              </v-btn>
            </v-col>
          </v-row>
          <div align="center" style="font-size: 21px;">ระยะที่</div>
          <v-card-text>
            <v-row class="justify-center">
              <v-col cols="3" class="text-center">
                <v-icon :color="isOn ? 'green' : 'grey'" style="font-size: 48px;">
                  mdi-lightbulb-on
                </v-icon>
                <div>ON</div>
              </v-col>
              <v-col cols="2" class="text-center">
                <v-icon
                  :color="numericValueState() === 1 || numericValueState() === 4 || numericValueState() === 5 ? 'red' : 'grey'"
                  style="font-size: 38px;">
                  mdi-lightbulb
                </v-icon>
              </v-col>
              <v-col cols="2" class="text-center">
                <v-icon
                  :color="numericValueState() === 2 || numericValueState() === 4 || numericValueState() === 5 ? 'green' : 'grey'"
                  style="font-size: 38px;">
                  mdi-lightbulb
                </v-icon>
              </v-col>
              <v-col cols="2" class="text-center">
                <v-icon
                  :color="numericValueState() === 3 || numericValueState() === 5 ? 'blue' : 'grey'"
                  style="font-size: 38px;">
                  mdi-lightbulb
                </v-icon>
              </v-col>
              <v-col cols="3" class="text-center">
                <v-icon :color="!isOn ? 'red' : 'grey'" style="font-size: 48px;">
                  mdi-lightbulb
                </v-icon>
                <div>OFF</div>
              </v-col>
            </v-row>

            <v-divider class="my-5"></v-divider>

            <v-row class="justify-center">
              <v-col cols="6" class="text-center">
                <v-icon :color="temperatureStatus() === 'cold' ||  temperatureStatus() === 'quite cold' ? 'green' : 'grey'"
                  style="font-size: 48px;">
                  mdi-air-conditioner
                </v-icon>
                <div>เครื่องปรับความร้อน</div>
              </v-col>
              <v-col cols="6" class="text-center">
                <v-icon :color="humidityStatus() === 'hot' ||  humidityStatus() === 'quite hot' ? 'green' : 'grey'"
                  style="font-size: 48px;">
                  mdi-water
                </v-icon>
                <div>เครื่องพ่นหมอก</div>
              </v-col>
            </v-row>

            <v-divider class="my-5"></v-divider>

            <v-row class="mt-5">
              <v-col cols="12" sm="6">
                <h3>อุณหภูมิ</h3>
                <h3 style="color: red;">{{ value_temp }} °C</h3>
                <h3>อยู่ในระดับที่ <h3 style="display:inline;color: red;font-size: 18px">{{ systemStatus }}</h3>
                </h3>
              </v-col>

              <v-col cols="12" sm="6">
                <h3>ความชื้น</h3>
                <h3 style="color: blue;">{{ value_humi }} %</h3>
                <h3>อยู่ในระดับที่ <span style="display:inline;color: blue;font-size: 18px">{{ systemStatus1 }}</span></h3>
              </v-col>
            </v-row>

            <v-divider class="my-5"></v-divider>

            <h3 class="text-center">ระยะที่ {{ numericValueState() }}</h3>

            <v-divider class="my-5"></v-divider>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
import mqtt from "mqtt";
definePageMeta({
  layouts: "iot",
});
export default {
  data() {
      return {
          value: 0,
          value1: 0,

          originalValue: 0,
          originalValue1: 0,
          isOn: false,
          value_temp: 0,
          value_humi: 0,
          value_state: 0,
          value_t1: 0,
          value_t2: 0,
          value_h1: 0,
          value_h2: 0,
          isOnlight: false,
          lightStates: [false, false, false], // Array to hold the state of each light
          lightStates1: [false, false, false], // Array to hold the state of each light
          stepIndex: 0, // To track the current step
          statustem: "",
          systemStatus: "",
          systemStatus1: "",
          value_control: "",
      };
  },
  watch: {
      value_temp(newVal) {
          this.temperatureStatus(); //เช็คว่าอุณหภูมิ ว่าอยู่ในระดับเหมาะสมรึไม่
          this.totalValueH(); //หาค่ากลาง
          this.numericValueState(); //ระยะที่ เท่าไร
          this.updateStatestemp(); //
      },
      value_humi(newVal) {
          this.humidityStatus();
          this.totalValueT();
          this.updateStateshumi(); //
      }
  },
  computed: {

  },

  created() {
      this.client = mqtt.connect('ws://broker.emqx.io:8083/mqtt');
      //this.client = mqtt.connect('ws://broker.hivemq.com:8000/mqtt');
      this.client.on('connect', this.onMqttConnect.bind(this));
      this.client.on('message', this.onMqttMessage.bind(this));

      this.scrollToButtons();
  },

  beforeDestroy() {
      this.client && this.client.end();
  },

  methods: {
      systemiot() {
          if (this.value_control == "1") {
              console.log("IsOn=1");
              this.isOn = true;
          } else {
              this.isOn = false;
              this.resetValues();
          }
      },
      scrollToButtons() {
          this.$nextTick(() => {
              const buttonSection = document.getElementById("button-section");
              if (buttonSection) {
                  buttonSection.scrollIntoView({ behavior: "smooth" });
              }
          });
      },
      start() {
          this.client.publish("control", '1');
          this.client.publish("init", '1');
          // this.isOn = true;
          this.value_temp = this.originalValue;
          this.value_humi = this.originalValue1;

          this.stepIndex = 0; // Reset step index when starting
      },
      stop() {
          // this.isOn = false;
          this.client.publish("init", '0');
          this.client.publish("control", '0');
          this.value_temp = 0;
          this.value_humi = 0;
          this.resetValues(); // Reset values when stopping
          this.lightStates = [false, false, false]; // Turn off all lights
          this.lightStates1 = [false, false, false]; // Turn off all lights
          this.value_t1 = 0;
          this.value_t2 = 0;
          this.value_h1 = 0;
          this.value_h2 = 0;
          this.value_state = "0";
          this.systemStatus = "ระบบกำลังรอข้อมูลเข้ามา";
          this.systemStatus1 = "ระบบกำลังรอข้อมูลเข้ามา";
      },

      resetValues() {
          this.value_temp = 0;
          this.value_humi = 0;
          this.lightStates = [false, false, false];
          this.lightStates1 = [false, false, false];
          this.value_t1 = 0;
          this.value_t2 = 0;
          this.value_h1 = 0;
          this.value_h2 = 0;
          this.value_state = "0";
          this.systemStatus = "ระบบกำลังรอข้อมูลเข้ามา";
          this.systemStatus1 = "ระบบกำลังรอข้อมูลเข้ามา";
      },
      onMqttConnect() {
          this.client.publish('op', 'status');
          this.client.subscribe('status');
          // this.client.subscribe('temperature');
          this.client.subscribe('moisture');
          this.client.subscribe('environment');// 
          this.client.subscribe('control');
      },
      onMqttMessage(topic, message) {
          if (topic === 'status') {
              this.msg = message.toString();
          }
          if (topic === 'environment' && this.isOn) {
              let data1 = JSON.parse(message.toString());
              this.value_temp = parseFloat(data1.temp);
              this.value_humi = parseFloat(data1.humi);
              this.value_state = parseInt(data1.state);
              this.value_t1 = parseFloat(data1.t1);
              this.value_t2 = parseFloat(data1.t2);
              this.value_h1 = parseFloat(data1.h1);
              this.value_h2 = parseFloat(data1.h2);
              // console.log("Temperature:", this.value_temp);
              // console.log("Humidity:", this.value_humi);
              // console.log("Temp min:", this.value_t1);
              // console.log("Temp max:", this.value_t2);
              // console.log("Humidity min:", this.value_h1);
              // console.log("Humidity max:", this.value_h2);

              // this.updateLightStates(); // อัปเดตสถานะไฟหลังจากได้รับข้อมูลอุณหภูมิ
          }

          if (topic === "control") {
              console.log("topic=", topic);
              this.value_control = message.toString();

              // let testini = message.toString();

              //  = init_data1;
              console.log("inile is ", this.value_control);
              this.systemiot()
          }
      },
      updateStateshumi() {
          const humiStatus = this.humidityStatus();
          // console.log(`tempStatus: ${tempStatus}, humiStatus: ${humiStatus}`);

          // กรณีที่ทั้งอุณหภูมิและความชื้นเหมาะสม
          if (humiStatus === 'appropriate') {
              this.lightStates1 = [false, true, false]; // Green only
              this.systemStatus1 = "เหมาะสม";
          }
          else if (humiStatus === 'dry') {
              this.lightStates1 = [true, false, false]; // Red only
              this.systemStatus1 = "แห้ง";
          } else if (humiStatus === 'damp') {
              this.lightStates1 = [false, false, true]; // Red only
              this.systemStatus1 = "ชื้น";
          }

          // // กรณีที่อุณหภูมิหรือความชื้นค่อนข้างเหมาะสม
          // else if (humiStatus === 'quite dry') {
          //     this.lightStates1 = [true, true, false]; // Red and Green

          //     this.systemStatus1 = "ค่อนข้างแห้ง";
          // } else if (humiStatus === 'quite damp') {
          //     this.lightStates1 = [false, true, true]; // Blue and Green
          //     this.systemStatus1 = "ค่อนข้างชื้น";
          // }
          // กรณีที่ทั้งอุณหภูมิและความชื้นไม่เหมาะสม
          else {
              this.lightStates1 = [true, true, true]; // All lights on
              this.systemStatus1 = "ไม่เหมาะสม";
          }
      },
      updateStatestemp() {
          const tempStatus = this.temperatureStatus();
          // console.log(`tempStatus: ${tempStatus}, humiStatus: ${humiStatus}`);
          // console.log(`tempStatus: ${tempStatus}`)
          if (tempStatus === 'appropriate') {
              this.lightStates = [false, true, false]; // Green only
              this.systemStatus = "เหมาะสม";
          } else if (tempStatus === 'hot') {
              this.lightStates = [true, false, false]; // Red only
              this.systemStatus = "ร้อนเกินไป";
          } else if (tempStatus === 'cold') {
              this.lightStates = [false, false, true]; // Red only
              this.systemStatus = "เย็นเกินไป";
          } 
          // else if (tempStatus === 'quite hot') {
          //     this.lightStates = [true, true, false]; // Red and Green
          //     this.systemStatus = "ค่อนข้างร้อน";
          // } else if (tempStatus === 'quite cold') {
          //     this.lightStates = [false, true, true]; // Blue and Green
          //     this.systemStatus = "ค่อนข้างเย็น";
          // }
      },


      cycleLights() {
          if (!this.isOn) return;

          const dataSets = [
              {
                  temp: "27.5",
                  humi: "85.175",
                  state: "1",
                  t1: "27.00",
                  t2: "28.00",
                  h1: "80.25",
                  h2: "90.10",
              },
              {
                  temp: "105.00",
                  humi: "3.175",
                  state: "2",
                  t1: "27.00",
                  t2: "28.00",
                  h1: "80.25",
                  h2: "90.10",
              },
              {
                  temp: "0.00",
                  humi: "100.175",
                  state: "3",
                  t1: "27.00",
                  t2: "28.00",
                  h1: "80.25",
                  h2: "90.10",
              },
              {
                  temp: "28.00",
                  humi: "81.175",
                  state: "4",
                  t1: "27.00",
                  t2: "28.00",
                  h1: "80.25",
                  h2: "90.10",
              },
              {
                  temp: "27.60",
                  humi: "87.175",
                  state: "5",
                  t1: "27.5",
                  t2: "28.00",
                  h1: "80.25",
                  h2: "90.10",
              },
              // {
              //     temp: "100.60",
              //     humi: "100.175",
              //     state: "6",
              //     t1: "27.5",
              //     t2: "28.00",
              //     h1: "80.25",
              //     h2: "90.10",
              // },
              // Reset to initial values on the 7th press
          ];

          if (this.stepIndex < dataSets.length) {
              const data = dataSets[this.stepIndex];
              // this.client.publish("temperature", JSON.stringify(data));
              this.value_temp = parseFloat(data.temp);
              this.value_humi = parseFloat(data.humi);
              this.value_state = data.state;
              this.value_t1 = parseFloat(data.t1);
              this.value_t2 = parseFloat(data.t2);
              this.value_h1 = parseFloat(data.h1);
              this.value_h2 = parseFloat(data.h2);

              this.stepIndex++;
              // this.updateLightStates();
          } else {
              this.resetValues();
              this.stepIndex = 0; // Reset the step index
          }
      },
      totalValue() {
          const ttotal = Number(this.value_temp) || 0;
          const htotal = Number(this.value_humi) || 0;
          return ttotal + htotal;
      },
      // New computed properties for the averages
      // คำนวณค่าเฉลี่ยอุณหภูมิ
      totalValueT() {
          const t1 = Number(this.value_t1) || 0;
          const t2 = Number(this.value_t2) || 0;
          return (t1 + t2) / 2;
      },
      // คำนวณค่าเฉลี่ยความชื้น
      totalValueH() {
          const h1 = Number(this.value_h1) || 0;
          const h2 = Number(this.value_h2) || 0;
          return Number((h1 + h2) / 2);
      },
      stepButtonLabel() {
          return `Step ${this.stepIndex + 1}`;
      },
      temperatureStatus() {
          //if (this.value_temp >= this.value_t1 && this.value_temp <= this.value_t2) return 'appropriate';
          // if (this.value_temp >= this.value_t1 && this.value_temp <= this.value_t2 && this.value_temp > (this.value_t1 + this.value_t2) / 2) return 'quite hot';
          // if (this.value_temp >= this.value_t1 && this.value_temp <= this.value_t2 && this.value_temp < (this.value_t1 + this.value_t2) / 2) return 'quite cold';
          //if (this.value_temp > this.value_t2) return 'hot';
          if (this.value_temp < this.value_t1) return 'cold';
          return 'inappropriate';
      },
      humidityStatus() {
          //if (this.value_humi >= this.value_h1 && this.value_humi <= this.value_h2) return 'appropriate';
          //if (this.value_humi >= this.value_h1 && this.value_humi <= this.value_h2 && this.value_humi < (this.value_h1 + this.value_h2) / 2) return 'quite dry';
          //if (this.value_humi >= this.value_h1 && this.value_humi <= this.value_h2 && this.value_humi > (this.value_h1 + this.value_h2) / 2) return 'quite damp';
          //if (this.value_humi < this.value_h1) return 'hot';
          if (this.value_temp > this.value_t2) return 'hot';
          return 'inappropriate';
      },
      numericValueState() {
          return Number(this.value_state);
      },
  },
};
</script>

<style scoped></style>

