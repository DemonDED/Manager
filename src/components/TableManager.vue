<template>
  <div id="table-component">
    <div class="tableUser">
      <button v-on:click="statusModal = true">Добавить</button>
      <table>
        <tr>
          <th style="cursor:pointer;" >Имя</th>
          <th style="cursor:pointer;" >Номер</th>
        </tr>
        <tr v-for="item in dataLocalStorage" v-bind:key="item.name" >
          <td>{{item.name}}
              <table>
                <tr v-for="items in item.employees" :key="items.name">
                  <td> + {{items.name}}</td>
                  <td>{{items.phone}}</td>
                </tr>
              </table>
          </td>
          <td>{{item.phone}}</td>
        </tr>
      </table>
    </div>
    <div class="addUser">
      <add-user  v-on:close="statusModal = false" v-if="statusModal" v-on:add-user="loadDataAddUser" v-bind:dataLocalStorage="dataLocalStorage"/>
    </div>
  </div>
</template>

<script>
import AddUser from './AddUser.vue' 

export default {
  data () {
    return {
      dataLocalStorage: [],
      statusModal: false,
    }
  },
  mounted () {
    this.loadData()
    console.log(this.dataLocalStorage[0])
  },
  methods: {
    loadData () {
      for (let i = 0; i < localStorage.length - 1; i++) {
        this.dataLocalStorage.push(JSON.parse(localStorage.getItem(i)))
      }
    },
    loadDataAddUser (dataFormAddUser) {
      
      for (let i = 0; i < localStorage.length - 1; i++) {
        if (dataFormAddUser.boss == this.dataLocalStorage[i].name) {
          this.dataLocalStorage[i].employees.push(dataFormAddUser)
          localStorage.removeItem(i)
          localStorage.setItem(i, JSON.stringify(this.dataLocalStorage[i]))
        }
      }
      this.dataLocalStorage.push(dataFormAddUser)
    }
  },
  components: {
    AddUser
  }
}
</script>

<style scoped>
  #table-component {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
  .tableUser, .addUser {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    align-content: center;
    width: 100%;
  }
  table {
    
    border-collapse: collapse;
  }
  td {
    border: 1px solid black;
    padding: 10px;
  }
</style>