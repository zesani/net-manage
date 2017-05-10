<template lang="html">
  <section class="hero">
    <div class="hero-body">
      <div class="container">
        <div class="level">
          <div class="level-left">
            <p class="level-item"><strong>Network</strong></p>
            <a class="button is-primary is-focused level-item"  @click="prompt">Add Device</a>
          </div>
        </div>
        <table class="table is-bordered" v-if="!(info === '')">
          <thead>
            <tr>
              <th>Device</th>
              <th>CPU</th>
              <th>TEMP</th>
              <th>In</th>
              <th>OUT</th>
              <th>xxx</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in info">
              <td>{{i.name}}</td>
              <td>{{i.cpu}}</td>
              <td>{{i.temp}}</td>
              <td>{{i.inbound}}</td>
              <td>{{i.outbound}}</td>
              <td @click="remove(i.name)">xx</td>
            </tr>
          </tbody>
        </table>
        <b-table :data="info">
          <template scope="props">
            <b-table-column field="name" label="Device Name" width="40">
                {{ props.name }}
            </b-table-column>
            <b-table-column field="cpu" label="CPU" width="40">
                {{ props.cpu }}
            </b-table-column>
            <b-table-column field="temp" label="TEMP" width="40">
                {{ props.temp}}
            </b-table-column>
            <b-table-column field="inbound" label="IN" width="40">
                {{ props.inbound}}
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  props: ['select'],
  data () {
    return {
    }
  },
  mounted () {
    this.select(0)
  },
  computed: {
    ...mapGetters(['info'])
  },
  methods: {
    ...mapActions(['addDevice', 'removeDevice']),
    remove (name) {
      this.$dialog.confirm({
        title: 'Deleting Device ' + name,
        message: 'Are you sure you want to <strong>delete</strong> your Device? This action cannot be undone.',
        confirmText: 'Delete Device',
        type: 'is-danger',
        onConfirm: () => {
          this.$toast.open({
            message: 'Device ' + name + ' deleted!'
          })
          this.removeDevice(name)
        }
      })
    },
    prompt () {
      var vm = this
      this.$dialog.prompt({
        message: `Device Name`,
        placeholder: 'router4503',
        maxlength: 20,
        inputMaxlength: 20,
        confirmText: `NEXT`,
        onConfirm: (value1) => {
          this.$dialog.prompt({
            message: `IP Address`,
            maxlength: 20,
            confirmText: `NEXT`,
            placeholder: 'router4503',
            onConfirm: (value2) => {
              this.$dialog.prompt({
                message: `Community`,
                maxlength: 20,
                placeholder: 'public',
                onConfirm: (value3) => {
                  this.$toast.open({
                    message: 'Device Name: ' + value1 + '     IP: ' + value2 + '     Community: ' + value3,
                    type: 'is-success',
                    duration: 3000
                  })
                  var params = new URLSearchParams()
                  params.append('ip', value2)
                  params.append('community', value3)
                  params.append('name', value1)
                  vm.addDevice(params)
                }
              })
            }
          })
        }
      })
    }
  }
}
</script>

<style lang="css">
</style>
