<template>
  <div id="app">
      <div v-if="!obj">
        <h3>Список объектов</h3>
        <table class="table">
          <thead>
          <tr>
            <th scope="col">id</th>
            <th scope="col">название</th>
            <th scope="col">дата создания</th>
            <th scope="col">количество свойств</th>
            <th scope="col">свойства</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="item in objs" :key="item.id">
            <td>{{item.id}}</td>
            <td>
              <a href="#"
                 :title="item.name"
                 @click.prevent="show(item.id)"
                 v-html="item.name"
              >
              </a>
            </td>
            <td>{{item.created_at}}</td>
            <td>{{item.properties.length}}</td>
            <td>
          <span v-for="prop in item.properties" :key="prop.id">
            ({{prop.name}}) {{prop.value}},
          </span>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
      <div v-else>
        <h3>{{obj.name}}</h3>
        <a href="#" @click.prevent="hide()">К списку объектов</a>
        <div style="margin-top: 20px">
          <div>id: {{obj.id}}</div>
          <div>название: {{obj.name}}</div>
          <div>дата создания: {{obj.created_at}}</div>
          <div>свойства:
            <span v-for="prop in obj.properties" :key="prop.id">
              {{prop.name}} - {{prop.value}},
            </span>
          </div>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      objs: null,
      obj: null
    };
  },
  methods: {
    show(id) {
      this.obj = this.objs.find( v => v.id = id);
    },
    hide() {
      this.obj = null;
    },
  },
  mounted() {
    this.axios
            .get('http://127.0.0.1/objects',{ withCredentials: false })
            .then(response => ( this.objs = response.data));
  }
}
</script>

<style>
  .table {
    width: 100%;
    max-width: 100%;
    margin-bottom: 1rem;
    background-color: transparent;
  }

  .table thead th {
    vertical-align: bottom;
    border-bottom: 2px solid #dee2e6;
  }

  table {
    border-collapse: collapse;
  }

  th {
    text-align: left;
  }

  .table td, .table th {
    padding: .75rem;
    vertical-align: top;
    border-top: 1px solid #dee2e6;
  }
</style>
