<template>
  <div>
    <input type="file" ref="fileInput" @change="handleFileChange" />
    <button @click="importXml">Import XML</button>
    <button @click="testLaravel">TestLaravel</button>
    <CategoryOffers />
  </div>
</template>

<script>
import axios from 'axios';
import CategoryOffers from "@/components/CategoryOffers.vue";


export default {
  name: 'App',
  components: {
    CategoryOffers
  },
  data(){
    return {
      file: null,
      currentComponent: null
    }
  },
  methods: {

    testLaravel(){
      axios.get('http://localhost:8000/api/test-connection')
          .then(response => {
            console.log(response.data)
          })
          .catch(error => {
            console.error(error)
          })
    },

    handleFileChange(event) {
      this.file = event.target.files[0];
    },

    importXml() {
      const formData = new FormData()
      formData.append('xml_file', this.file)

      axios.post('http://localhost:8000/api/import-xml', formData)
          .then(response => {
            console.log(response.data)
          })
          .catch(error => {
            console.error(error)
          })
    }
  },
  mounted() {
    this.currentComponent = CategoryOffers
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
