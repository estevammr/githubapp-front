<template>
  <div id="app">
  <div class="search-wrapper">
    <input type="text" v-model="search" placeholder="Search repo.."/>
        <label>Search repo:</label>
  </div>

  <div class="container">
    <div class="row">
      <div class="col-md-6 col-sm-8 col-xs-12 col-md-offset-3 col-sm-offset-2">
    <div class="card" v-for="d in filteredList">
        <div class="text">
          <a v-bind:href="d.clone_url" target="_blank">
            <p>url: {{ d.clone_url }}</p>
          </a>
          <p>stars: {{ d.stargazers_count }}</p>
          <p>score: {{ d.score }}</p>
          <p>id: {{ d.node_id }}</p>
          <p>name: {{ d.name }}</p>
          <p>languages: {{ d.languages_url }}</p>
          <p>homepage: {{ d.homepage }}</p>
          <p>git url: {{ d.git_url }}</p>
          <p>full name: {{ d.full_name }}</p>
          <p>forks: {{ d.forks }}</p>
        </div>
    </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      data: [],
      search: ''
    }
  },
  mounted () {
    axios.all([
      axios.get('https://warm-woodland-36198.herokuapp.com/api/gits?language=elixir'),
      axios.get('https://warm-woodland-36198.herokuapp.com/api/gits?language=java'),
      axios.get('https://warm-woodland-36198.herokuapp.com/api/gits?language=ruby'),
      axios.get('https://warm-woodland-36198.herokuapp.com/api/gits?language=lua'),
      axios.get('https://warm-woodland-36198.herokuapp.com/api/gits?language=python')
    ]).then(axios.spread((elixir, java, ruby, lua, python) => {
      this.data = elixir.data.concat(java.data)
      this.data = this.data.concat(ruby.data)
      this.data = this.data.concat(lua.data)
      this.data = this.data.concat(python.data)
    }, error => {
      console.error(error)
    }))
  },
  methods: { },
  computed: {
    filteredList () {
      return this.data.filter(d => {
        return d.clone_url.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>

<style scoped>

.card {
  display: inline-block;
  box-shadow: 0 1px 2px 0 rgba(0,0,0,.15);
  margin: 20px;
  position: relative;
  margin-bottom: 50px;
  transition: all .2s ease-in-out;
}

.card:hover {
  /*box-shadow: 0 5px 22px 0 rgba(0,0,0,.25);*/
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
  margin-bottom: 54px;
}

.text {
  background: #FFF;
  padding: 20px;
  min-height: 200px;
}

.text p {
  margin-bottom: 0px;
}

</style>
