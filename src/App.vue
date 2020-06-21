<template>
  <div id="app">
    <header>
      <div class="container">
        <a href="/" class="logo"><h1>Облако знаний</h1></a>
      </div>
    </header>
    <main>
      <div class="container">
        <h1 class="title">Витрина</h1>
        <div class="currency">
          <h4>Выводить стоимость:</h4>
          <label>
            <input type="radio" name="currency" value="rub" v-model="currency" checked> в рублях
          </label>
          <br>
          <label>
            <input type="radio" name="currency" value="bonus" v-model="currency"> в бонусах
          </label>
        </div>
        <div class="filter">
          <filtered-items ref="filter"></filtered-items>
        </div>
        <catalog ref="cata"></catalog>
      </div>
    </main>
  </div>
</template>

<script>
import catalog from './components/Catalog'
import filter from './components/Filter'

export default {
  name: 'App',
  data() {
    return {
      currency: 'rub'
    }
  },
  methods: {
      postJSON() {
          return fetch('http://krapipl.imumk.ru:8082/api/mobilev1/update', {
              method: 'POST',
              headers: {
                  "Content-Type": "text/html"
              },
              body: { "data": "" }
          })
              .then(result => result.json())
              .catch(err => {
                  this.$root.$refs.error.errorShow = true
                  console.error(err)
              })
      }
  },
  components: {
      catalog,
      'filtered-items': filter
  }
}
</script>

<style>
    @import './styles/style.css';
</style>