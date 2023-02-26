<template>
    <div class="modal" >
        <div class="modal__container">
            <form class="modal__form" v-if="loaded" @submit.prevent="deleteProduct">
                <h3>Deseja excluir o produto: {{ product.data.name }}?</h3>

                <div class="form__group">
                    <button class="form__button" type="submit">Sim</button>
                    <button class="form__button" @click="cancelAction">Não</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>


export default {
    middleware: ['verified'],
    data: () => ({
        loaded: false,
        product: {
            data: {
                name: '',
                url_name: '',
                category_id: '',
                description: '',
            }
        }
    }),
    methods: {

        async deleteProduct() {

            try {

                await this.$axios.get('/sanctum/csrf-cookie')

                const product = await this.$axios.delete('/api/products/' + this.product.data.id)
                
                this.$parent.refreshData(product)

                this.$router.push({ name: 'product'})
            } catch (e) {

            }
        },

        cancelAction() {
            return this.$router.push({ name: 'product'})
        }
    },
    async mounted() {
        try{
            if(!(this.$route.params.product.id)){
                return this.$router.push({ name: 'product'})
            }

            this.product = await this.$axios.get('/api/products/' + this.$route.params.product.id)

            this.loaded = true;
        } catch (e){

        }
    },
}
</script>

<style scoped>
    .modal{
        position: absolute;
        width: 100vw;
        height: 100vh;
        top: 0;
        left: 0;
        background-color: rgba(0, 0, 0, 0.15);
    }

    .modal__container{
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modal__form{
        padding: 20px;
        background-color: white;
        width: 350px;
    }

    .form__input{
        width: 93%;
    }
</style>