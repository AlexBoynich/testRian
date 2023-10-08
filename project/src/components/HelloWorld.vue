<template>
  <div class="hello">
    <div 
    v-for="(category, index) in categories"
    :class="`col${index}`"
    :key="category.id"
    @drop="onDrop($event, category.id)"
    @dragover.prevent
    @dragenter.prevent
    >    
    <h1 class="title">{{category.title}}</h1>
    <div
     v-for="(product, index) in productsList.filter(x => x.categoryId===category.id)"
     @dragstart="onDragStart($event, product)"
     >
      <Product
      :product="product"
      :index="index"
      @deleteProduct="deleteProduct"
      draggable="true"
      @addProduct="changeProductsList"
      ref="productComponent"
      />
    </div>
    </div>
  </div>
</template>

<script lang="ts">
import Product from './Product.vue';

export default {
  name: 'HelloWorld',
  props: ['productsList'],
  components: {
    Product
  },
  data() {
    return {
      categories: [
        {
          id: 0,
          title: 'Необработанные'
      },
      {
          id: 1,
          title: 'В работе'
      },
      {
          id: 2,
          title: 'Завершенные'
      }
    ]
    }
  },
  methods: {
    deleteProduct(index) {
      this.$emit('deleteProduct', index)
    },
    onDragStart(e, item) {
      e.dataTransfer.dropEffect='move'
      e.dataTransfer.effectAllowed='move'
      e.dataTransfer.setData('itemId', item.id.toString()
      )
    },
    closeChildModal() {
     
    },
    onDrop(e, categoryId) {
      const itemId = e.dataTransfer.getData('itemId')
      let result = this.productsList.map( (x) => {
        if (x.id==itemId) {
          x.categoryId = categoryId
        }
        return x
      })
      this.$emit('changeProductsList', result)
    },
    changeProductsList(prod) {
      this.$emit('changeProductsList', prod )
    }
  },
  computed: {
    
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
.hello
  width: 100%
  display: grid
  grid-template-columns: 1fr 1fr 1fr
  grid-gap: 20px
  padding: 20px
  .col0, .col1, .col2
    width: 30vw
    border-radius: 10px
    @media (max-width: 800px)
      width: 26vw
  .col0
    background: #bae2ff
  .col1
    background: #ffe2ab
  .col2
    background: #ffc7c7
  .title
    text-align: center
    @media (max-width: 1000px)
      font-size: 20px
      @media (max-width: 600px)
        font-size: 14px
</style>
