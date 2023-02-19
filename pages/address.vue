<template>
    <div>
        <header>
            <Navbar />
        </header>
        <h1>Meus Endereços</h1>

        <ul>
            <li>{{ address.data?.cidade }} - {{ address.data?.bairro }}, {{ address.data?.rua }}, Nº {{ address.data?.numero }}</li>
        </ul>
    </div>
</template>

<script>
    export default {
        middleware: ['verified'],
        data: () => ({
            address: []
        }),
        async mounted() {
            try{
                this.address = await this.$axios.get('api/address')
            } catch (e){
                
            }
        },
    }
</script>

<style>
</style>