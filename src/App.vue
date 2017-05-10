<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <div class="tabs menu-bar">
          <ul>
            <li :class="selectMenu[0].style" @click="select(0)"><router-link to="/">Monitor</router-link></a></li>
            <li :class="selectMenu[1].style" @click="select(1)"><router-link to="/bandwidth">Bandwidth</router-link></li>
          </ul>
        </div>
      </div>
    </section>
    <router-view :select="select"></router-view>
    <ul>
      <li v-for="i in InFire">{{i.name}}---{{i.inbound}}---{{i.outbound}}</li>
    </ul>
  </div>
</template>

<script>
var firebase = require('firebase')
var config = {
  apiKey: 'AIzaSyBe7u9FtZ5ATGU69hfd9NBbHiGCGreSWIM',
  authDomain: 'snmp-monitor-fitm.firebaseapp.com',
  databaseURL: 'https://snmp-monitor-fitm.firebaseio.com',
  projectId: 'snmp-monitor-fitm',
  storageBucket: 'snmp-monitor-fitm.appspot.com',
  messagingSenderId: '992008104190'
}
firebase.initializeApp(config)
import { mapActions } from 'vuex'
export default {
  name: 'app',
  data () {
    return {
      selectMenu: [{
        style: ''
      },
      {
        style: ''
      },
      {
        style: ''
      },
      {
        style: ''
      }],
      InFire: []
    }
  },
  methods: {
    ...mapActions(['getInfo', 'getInfoFire']),
    select (menu) {
      this.selectMenu[0].style = ''
      this.selectMenu[1].style = ''
      this.selectMenu[2].style = ''
      this.selectMenu[3].style = ''
      this.selectMenu[menu].style = 'is-active'
    }
  },
  mounted () {
    var vm = this
    this.select(0)
    // var Info = firebase.database().ref('/info')
    // Info.on('child_changed', function (snapshot) {
    //   let indexInfo = vm.InFire.find(item => item.key === snapshot.key)
    //   let data = snapshot.val()
    //   data.key = snapshot.key
    //   if (indexInfo) {
    //     indexInfo.cpu = data.cpu
    //     indexInfo.temp = data.temp
    //     indexInfo.inbound = data.inbound
    //     indexInfo.outbound = data.outbound
    //     indexInfo.interface = data.interface
    //   }
    // })
    // Info.on('child_added', function (snapshot) {
    //   let indexInfo = vm.InFire.findIndex(item => item.key === snapshot.key)
    //   let data = snapshot.val()
    //   data.key = snapshot.key
    //   if (indexInfo === -1) {
    //     console.log('add')
    //     vm.InFire.push(data)
    //   }
    // })
    setInterval(() => {
      vm.getInfo()
      // vm.getInfoFire()
    }, 2000)
  }
}
</script>

<style>
</style>
