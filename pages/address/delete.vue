<template>
    <div class="modal" >
        <div class="modal__container">
            <form class="modal__form" v-if="loaded" @submit.prevent="deleteAddress">
                <h3>Deseja excluir o endereço: {{ address.data.city }} - {{ address.data.state }}, {{ address.data.street }},  Nº {{ address.data.number }} - {{ address.data.postal_code }}?</h3>

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
        address: {
            oldCep: '',
            data: {
                postal_code: '',
                state: '',
                city: '',
                neighborhood: '',
                street: '',
                number: '',
                complement: '',
            }
        }
    }),
    methods: {

        async deleteAddress() {

            try {

                await this.$axios.get('/sanctum/csrf-cookie')

                const address = await this.$axios.delete('/api/address/' + this.address.data.id)
                
                this.$parent.refreshData(address)

                this.$router.push({ name: 'address'})
            } catch (e) {

            }
        },

        cancelAction() {
            return this.$router.push({ name: 'address'})
        }
    },
    async mounted() {
        

        try{
            if(!(this.$route.params.address.id)){
                return this.$router.push({ name: 'address'})
            }

            this.address = await this.$axios.get('/api/address/' + this.$route.params.id)

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