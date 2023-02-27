<template>
    <div>
        <header>
            <Navbar />
        </header>
        <section v-if="loaded">
            <h1>Meus Endereços</h1>

            <nuxt-link :to="{ name: 'address-create' }"> > Criar Endereço </nuxt-link>
            
            <div v-for="address in addresses.data">
                <li>{{ address.city }} - {{ address.neighborhood }}, {{ address.street }}, Nº {{ address.number }}<div><nuxt-link :to="{ name: 'address-edit', params: {id: address.id}}">Editar</nuxt-link><nuxt-link :to="{ name: 'address-delete', params: {id: address.id}}">Excluir</nuxt-link></div></li>
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
            addresses: {
                data:{}
            }
        }),

        methods: {
            refreshData(address) {
                this.addresses.data = address.data
            },
        },
        async mounted() {

            try{

                this.addresses = await this.$axios.get('/api/address')

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