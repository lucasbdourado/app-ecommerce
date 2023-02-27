<template>
    <div class="modal">
        <div class="modal__container">
            <form @submit.prevent="createAddress" class="modal__form">
                <div class="form__group">
                    <label class="form__label" for="postal_code">CEP</label>
                    <input class="form__input" type="text" name="postal_code" v-on:focus="getCEP" v-on:blur="handleCEP" id="cep"
                        v-model="address.data.postal_code">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="state">UF</label>
                    <input class="form__input" type="text" name="state" id="state" v-model="address.data.state">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="city">Cidade</label>
                    <input class="form__input" type="text" name="city" id="city" v-model="address.data.city">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="neighborhood">Bairro</label>
                    <input class="form__input" type="text" name="neighborhood" id="neighborhood" v-model="address.data.neighborhood">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="street">Rua</label>
                    <input class="form__input" type="text" name="street" id="street" v-model="address.data.street">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="number">Numero</label>
                    <input class="form__input" type="text" name="number" id="number" v-model="address.data.number">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="complement">Complemento</label>
                    <input class="form__input" type="text" name="complement" id="complement"
                        v-model="address.data.complement">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <button class="form__button" type="submit">Criar</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>


export default {
    middleware: ['verified'],
    data: () => ({
        address: {
            oldPostalCode: '',
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

        async handleCEP(event) {

            if (!this.validateCEP(event.target.value)) return

            if (event.target.value == this.address.oldPostalCode) return

            const addressData = await fetch('https://viacep.com.br/ws/' + event.target.value + '/json', {
                method: 'GET'
            }).then(function (res) {
                if (res.status != 200) {
                    throw response.status;
                } else {
                    return res.json()
                }
            });

            this.address.data.state = addressData.uf
            this.address.data.city = addressData.localidade
            this.address.data.neighborhood = addressData.bairro
            this.address.data.street = addressData.logradouro

            this.$forceUpdate();
        },

        validateCEP(postalCode) {
            const regex = /^[0-9]{5}[-]?[0-9]{3}$/;

            if (!(regex.test(postalCode))) {

                return false
            } else {

                return true
            }
        },

        getCEP(event) {

            this.address.oldPostalCode = event.target.value
        },

        async createAddress() {

            try {

                await this.$axios.get('/sanctum/csrf-cookie')

                const address = await this.$axios.post('/api/address', this.address.data)
                
                this.$parent.refreshData(address)

                this.$router.push({ name: 'address'})
            } catch (e) {

            }
        }
    }
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