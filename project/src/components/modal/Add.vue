<template>
    <div>
        <div 
        @click="$emit('showModal')"
        class="overlay">
    </div>
    <div class="addModal">
        <h1>Можно ничего не заполнять, а просто нажать кнопку</h1>
        <input type="text" v-model="prod.category" placeholder="Категория">
        <input type="text" v-model="prod.description" placeholder="Описание">
        <input type="text" v-model="prod.image" placeholder="Картинка">
        <input type="number" v-model="prod.price" placeholder="Цена">
        <input type="text" v-model="prod.title" placeholder="Заголовок">
        <input type="number" v-model="prod.rating.rate" placeholder="Рейтинг">
        <input type="number" v-model="prod.rating.count" placeholder="Количество продукта">
        <button
        @click="addProduct()"
            class="button">Добавить</button>
    </div>
    </div>
</template>

<script>    


export default {
    data() {
        return {
            prod: {
                category: '',
                description: '',
                image: '',
                price: null,
                id: null,
                rating: {
                    rate: null,
                    count: null,
                },
                title: ''
            }
        }
    },
    methods: {
        addProduct() {
            this.$emit('addProduct',this.prod );
            
        }
    },
    computed: {

    },
    props: ['currentProduct'],
    mounted (){
        this.prod.category = (this.currentProduct) ? this.currentProduct.category : '';
        this.prod.description = (this.currentProduct) ? this.currentProduct.description : '';
        this.prod.image = (this.currentProduct) ? this.currentProduct.image : '';
        this.prod.price = (this.currentProduct) ? this.currentProduct.price : null;
        this.prod.title = (this.currentProduct) ? this.currentProduct.title : '';
        this.prod.rating.rate = (this.currentProduct) ? this.currentProduct.rating.rate : null;
        this.prod.rating.count = (this.currentProduct) ? this.currentProduct.rating.count : null;
        this.prod.id = (this.currentProduct) ? this.currentProduct.id : null
    }

}
</script>

<style scoped lang="sass">
.overlay
    display: flex
    align-items: center
    justify-content: center
    width: 100vw
    height: 100vh
    position: fixed
    background: black
    opacity: 0.3
    top: 0
    left: 0
    cursor: pointer
.addModal
    display: flex
    position: fixed
    align-items: center
    justify-content: center
    top: 25vh
    left: 25vw
    z-index: 10
    width: 50vw
    flex-direction: column
    padding: 50px
    background: white
    opacity: 1
    gap: 20px
    border-radius: 10px
    input
        border-radius: 10px
        width: 70%
.button
    width: 100px
    background: #bae2ff
    border: none
    border-radius: 10px
    cursor: pointer
    padding: 5px 10px
</style>