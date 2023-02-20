<template>
    <div>
        <header>
            <Navbar />
        </header>
        <section v-if="loaded">
            <h1>Meus Endereços</h1>

            <nuxt-link :to="{ name: 'address-create' }"> > Criar Endereço </nuxt-link>
            
            <div v-for="add in address.data">
                <li>{{ add.cidade }} - {{ add.bairro }}, {{ add.rua }}, Nº {{ add.numero }}<div><nuxt-link :to="{ name: 'address-edit', params: {address: add}}">Editar</nuxt-link><nuxt-link :to="{ name: 'address-delete', params: {address: add}}">Excluir</nuxt-link></div></li>
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
            address: {
                data:{}
            }
        }),

        methods: {
            refreshData(address) {
                this.address.data = address.data
            },
        },
        async mounted() {

            try{

                this.address = await this.$axios.get('/api/address')

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