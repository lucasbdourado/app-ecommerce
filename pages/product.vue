<template>
    <div>
        <header>
            <Navbar />
        </header>
        <section v-if="loaded">
            <h1>Meus Produtos</h1>

            <nuxt-link :to="{ name: 'product-create' }"> > Criar Produto </nuxt-link>
            
            <div v-for="product in products.data">
                <li>{{ product.name }} - <nuxt-link :to="{ name: 'product-edit', params: {id: product.id}}">Editar</nuxt-link><nuxt-link :to="{ name: 'product-delete', params: {id: product.id}}">Excluir</nuxt-link> <div v-for="image in product.images"><img :src="`http://localhost:8000/storage/${image.url_name}`" alt="" srcset="" height="150px" width="150px"></div></li>
            </div>
            <NuxtChild/>
        </section>
    </div>
</template>

<script>
    export default {

        middleware: ['verified'],

        data: () => ({
            loaded: false,
            products: {
                data:{}
            }
        }),

        methods: {
            refreshData(products) {
                this.products.data = products.data
            },
        },
        async mounted() {

            try{

                this.products = await this.$axios.get('/api/products')

                this.loaded = true;
            } catch (e){

            }
        },
    }
</script>

<style>

    li{
        display: flex;
        gap: 10px;
    }

</style>