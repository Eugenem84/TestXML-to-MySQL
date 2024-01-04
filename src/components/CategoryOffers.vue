<script >
import axios from "axios";
export default {
  components: {

  },

  data(){
    return {
      categories: [],
      subCategories: [],
      selectedCategory: null,
      selectedSubCategory: null,
      offers:[],
    }
  },

  created() {
    this.loadCategories()
  },

  methods: {
    async loadCategories(){
      try {
        const response = await axios.get('http://localhost:8000/api/categories')
        this.categories = response.data.categories
        console.log('категории загружены', this.categories)
      } catch (error) {
        console.error('ошибка при загрузке категорий',error)
      }
    },
    async loadSubCategories(categoryId){
      try {
        const response = await axios.get('http://localhost:8000/api/subcategories/' + encodeURIComponent(categoryId))
        this.subCategories = response.data.subCategories
        console.log('подкатегории загружены', this.subCategories)
      } catch (error) {
        console.error('ошибка при загрузке подкатегорий', error)
      }
    },
    async loadOffers(subCategoryId){
      try {
        const response = await axios.get('http://localhost:8000/api/offers/' + encodeURIComponent(subCategoryId))
        this.offers = response.data.offers
        console.log('оферы загружены', this.offers)
      } catch (error) {
        console.error(error)
      }
    }
  }
}
</script>

<template>

  <div id="categories">
    <h3>Выбор категории</h3>
    <select v-model="selectedCategory" @change="loadSubCategories(selectedCategory)">
      <option v-for="category in categories" :key="category.id" :value="category.id">
        {{ category.name }}
      </option>
    </select>

    <h3>Выбор подкатегории</h3>
    <select v-model="selectedSubCategory" @change="loadOffers(selectedSubCategory)" >
      <option v-for="subCategory in subCategories" :key="subCategory.id" :value="subCategory.id">
        {{ subCategory.name }}
      </option>
    </select>
  </div>

  <div id="offers">
    <h3>Товары</h3>
    <ul>
      <li v-for="offer in offers" :key="offer.id">
        <div class="offer-container" >
          <h5>{{ offer.name }}</h5>
          <img :src="offer.picture" width="100" />
          <p>новая цена: {{ offer.price }} {{offer.currencyId}}</p>
          <p>старая цена: {{ offer.old_price}} {{offer.currencyId}}</p>
          <p>id: {{ offer.id }}</p>

          <a :href=" offer.url " class="offer-link">подробнее</a>
        </div>
      </li>
    </ul>
  </div>

</template>

<style scoped>

</style>