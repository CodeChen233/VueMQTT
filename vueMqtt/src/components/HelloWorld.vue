<template>
  <div id="app">
    <p>mqtt收到的数据：</p>
    <p>{{this.msg}}</p>
  </div>
</template>

<script>

  import mqtt from 'mqtt'

  var client
  const options = {
    connectTimeout: 40000,
    clientId: '',
    username: 'admin',
    password: 'admin',
    clean: true
  }
  client = mqtt.connect('ws://120.79.57.0:8083',options)
  export default {
    data() {
      return {
        msg: '--'
      }
    },

    created() {
      this.mqttMsg()
    },

    methods: {
      mqttMsg() {
        client.on('connect', (e) => {
          console.log("连接成功！！！")
          client.subscribe('data/receive', { qos: 0 }, (error) => {
            if (!error) {
              console.log('订阅成功')
            } else {
              console.log('订阅失败')
            }
          })

        })
        // 接收消息处理
        client.on('message', (topic, message) => {
          console.log('收到来自', topic, '的消息', message.toString())
          this.msg = message.toString()
        })
      }
    }


  }
</script>
<style scoped>
</style>

