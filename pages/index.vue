<template>
    <div>
        <header>
            <Navbar />
        </header>
        <main>
            <h1>Produtos</h1>
            <div v-for="product in products.data">
                <div>
                    <div>
                        <img :src="`http://localhost:8000/storage/${product.images[0]?.url_name}`" alt="" srcset="" height="150px" width="150px">
                    </div>

                    <h4>{{ product.name }}</h4>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
    export default {
        data: () => ({
            loaded: false,
            products: {
                data:{}
            }
        }),

        methods: {},

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

</style>