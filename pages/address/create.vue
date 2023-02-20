<template>
    <div class="modal">
        <div class="modal__container">
            <form @submit.prevent="createAddress" class="modal__form">
                <div class="form__group">
                    <label class="form__label" for="cep">CEP</label>
                    <input class="form__input" type="text" name="cep" v-on:focus="getCEP" v-on:blur="handleCEP" id="cep"
                        v-model="address.data.cep">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="uf">UF</label>
                    <input class="form__input" type="text" name="uf" id="uf" v-model="address.data.uf">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="cidade">Cidade</label>
                    <input class="form__input" type="text" name="cidade" id="cidade" v-model="address.data.cidade">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="bairro">Bairro</label>
                    <input class="form__input" type="text" name="bairro" id="bairro" v-model="address.data.bairro">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="rua">Rua</label>
                    <input class="form__input" type="text" name="rua" id="rua" v-model="address.data.rua">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="numero">Numero</label>
                    <input class="form__input" type="text" name="numero" id="numero" v-model="address.data.numero">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="complemento">Complemento</label>
                    <input class="form__input" type="text" name="complemento" id="complemento"
                        v-model="address.data.complemento">
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
            oldCep: '',
            data: {
                cep: '',
                uf: '',
                cidade: '',
                bairro: '',
                rua: '',
                numero: '',
                complemento: '',
            }
        }
    }),
    methods: {

        async handleCEP(event) {

            if (!this.validateCEP(event.target.value)) return

            if (event.target.value == this.address.oldCep) return

            const addressData = await fetch('https://viacep.com.br/ws/' + event.target.value + '/json', {
                method: 'GET'
            }).then(function (res) {
                if (res.status != 200) {
                    throw response.status;
                } else {
                    return res.json()
                }
            });

            this.address.data.uf = addressData.uf
            this.address.data.cidade = addressData.localidade
            this.address.data.bairro = addressData.bairro
            this.address.data.rua = addressData.logradouro

            this.$forceUpdate();
        },

        validateCEP(cep) {
            const regex = /^[0-9]{5}[-]?[0-9]{3}$/;

            if (!(regex.test(cep))) {

                return false
            } else {

                return true
            }
        },

        getCEP(event) {

            this.address.oldCep = event.target.value
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