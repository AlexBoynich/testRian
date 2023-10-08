<template>
  <div id="app container">
    <button
     @click="showModal"
      class="button"
      >Добавить продукт</button>
    <HelloWorld 
    :productsList="productsList"
    @deleteProduct="deleteProduct"
    @changeProductsList="changeProductsList"
    ref="helloWorld"
    />
    <Add
    @showModal="showModal"
    @addProduct="addProduct"
    v-if="modal"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'
import Add from './components/modal/Add.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    Add
  },
  data() {
    return {
      productsList: [],
      modal: false
    }
  },
  methods: {
    showModal() {
      this.modal = !this.modal
    },
    loadProducts() {
      axios.get('https://fakestoreapi.com/products')
    .then((list) => {
      this.productsList = list.data
      .map((v)=> {
        v.categoryId=0
        return v
      })
      .sort(function(a, b) {
        if (a.rating.rate > b.rating.rate ) {
          return 1
        } 
        if (a.rating.rate < b.rating.rate ) {
          return -1
        } 
        return 0
      })
    })
    },
    deleteProduct(id) {
      this.productsList.map((el, index)=> {
        if (el.id === id) {
          this.productsList.splice(index, 1)
          axios.delete(`https://fakestoreapi.com/products/${index}`)
        }
      })
    },
    addProduct(category, description, image, price, title, rate) {
      axios.post('https://fakestoreapi.com/products',{
  
                    title: 'test product',
                    price: 13.5,
                    description: 'lorem ipsum set',
                    image: 'https://i.pravatar.cc',
                    category: 'electronic',
                    rating: {
                      rate: 0,
                      count: 2000
                    }  
        })
            .then((res) => {
              res.data.categoryId = 0
              this.productsList.push(res.data)
        })
        this.showModal()
    },
    closeChildModal() {
      this.$refs.helloWorld.closeChildModal()
    },
    changeProductsList(prod) {
      this.productsList.map(el=> {
        if(el.id==prod.id) { 
          el.category=prod.category;
          el.description=prod.description;
          el.image=prod.image;
          el.price=Number(prod.price);
          el.title=prod.title;
          el.rating.rate=Number(prod.rating.rate);
          el.rating.count=Number(prod.rating.count)
        }
        return 
      })
      return this.closeChildModal()
    }
  },
  mounted() {
    this.loadProducts()
  }

}
</script>

<style scoped lang="sass">
*
    margin: 0
    padding: 0
    box-sizing: border-box
.container
  width: 100%
  max-width: 2000px
.button
    width: 100px
    background: #bae2ff
    border: none
    border-radius: 10px
    cursor: pointer
    padding: 5px 10px
    position: fixed
    top: 5px
    left: 5px
</style>
