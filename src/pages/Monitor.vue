<template lang="html">
  <section class="hero">
    <div class="hero-body">
      <div class="container">
        <div class="columns  is-multiline">
          <div class="column is-8 is-offset-2">
            <div class="level">
              <div class="level-left">
                <p class="level-item"><strong>Network</strong></p>
                <a class="button is-primary is-focused level-item"  @click="prompt">Add Device</a>
              </div>
            </div>
          </div>
          <div class="column is-8 is-offset-2 is-hidden-mobile">
            <table class="table is-bordered" v-if="!(info === '')">
              <thead>
                <tr>
                  <th>Device</th>
                  <th>MEMMORY</th>
                  <th>CPU</th>
                  <th>TEMP</th>
                  <th>IN</th>
                  <th>OUT</th>
                  <th>DELETE</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="i in info">
                  <td>{{i.name}}</td>
                  <td></td>
                  <td>{{i.cpu}}</td>
                  <td>{{i.temp}}</td>
                  <td>{{i.inbound}}</td>
                  <td>{{i.outbound}}</td>
                  <td width="10"><a class="button is-danger" @click="remove(i.name)">DELETE</a></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="is-hidden-tablet" v-if="!(info === '')">
          <table class="table is-bordered" v-for="i in info">
            <tbody>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item level-left">
                      <p class=""><b>Device</b></p>
                    </div>
                    <div class="level-item level-right">
                      <p class="">{{i.name}}</p>
                    </div>
                  </div>
                </td>
              </tr>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item level-left">
                      <p class=""><b>CPU</b></p>
                    </div>
                    <div class="level-item level-right">
                      <p class="">{{i.cpu}}</p>
                    </div>
                  </div>
                </td>
              </tr>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item level-left">
                      <p class=""><b>TEMP</b></p>
                    </div>
                    <div class="level-item level-right">
                      <p class="">{{i.temp}}</p>
                    </div>
                  </div>
                </td>
              </tr>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item level-left">
                      <p class=""><b>IN</b></p>
                    </div>
                    <div class="level-item level-right">
                      <p class="">{{i.inbound}}</p>
                    </div>
                  </div>
                </td>
              </tr>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item level-left">
                      <p class=""><b>OUT</b></p>
                    </div>
                    <div class="level-item level-right">
                      <p class="">{{i.outbound}}</p>
                    </div>
                  </div>
                </td>
              </tr>
              <tr>
                <td>
                  <div class="level is-mobile">
                    <div class="level-item has-text-centered">
                      <p class=""><a class="delete is-large" @click="remove(i.name)"></a></p>
                    </div>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
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
      selected: {}
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
