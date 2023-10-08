<template>
    <div
    @dragstart="$emit('onDragStart', $event, product)" 
    class="product">
        <h2>{{ product.title }}</h2>
        <p>{{ product.description }}</p>
        <img :src=product.image alt="картинка" class="productImage">
        <h4> Rate: {{ (product.rating && product.rating.rate) ?  product.rating.rate.toFixed(1) : '' }}</h4>
        <div class="buttons">
            <button
            class="button"
            @click="showModal()"
            >Редактировать продукт
            </button>
            <button
            class="button"
            @click="$emit('deleteProduct', product.id)">Удалить продукт</button>
        </div>
        <Add
        :currentProduct="product"
        @addProduct="addProduct"
        v-if="modal"
        @showModal="showModal"
        />
    </div>
</template>

<script>
import Add from './modal/Add.vue';

export default {
    props: ['product', 'index'],
    components: {
        Add
    },
    data() {
        return {
            modal: false
        }
    },
    methods: {
        showModal() {
            this.modal=!this.modal
        },
        addProduct(prod) {
            this.$emit('addProduct', prod)
        },
        closeChildModal() {
            this.modal=!this.modal
        }
    }
}
</script>

<style scoped lang="sass">
.product
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center
    margin: 20px
    border-radius: 10px
    background: white
    padding: 20px
    .productImage
        max-width: 80%
    .buttons
        display: flex
        gap: 20px
        @media (max-width: 1000px)
            flex-direction: column

    .button
        width: 100px
        background: #bae2ff
        border: none
        border-radius: 10px
        cursor: pointer
        padding: 5px 10px
@media (max-width: 1000px)
    h2
        font-size: 20px
        max-width: 100%
        overflow: hidden
        @media (max-width: 600px)
            font-size: 14px
    h4
        font-size: 20px
    p
        font-size: 10px
</style>