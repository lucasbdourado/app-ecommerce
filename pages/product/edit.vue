<template>
    <div class="modal" v-if="loaded">
        <div class="modal__container">
            <form @submit.prevent="updateProduct" class="modal__form">
                <div class="form__group">
                    <label class="form__label" for="name">Nome do Produto</label>
                    <input class="form__input" type="text" name="name" id="name" v-model="product.data.name">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="category_id">Categoria</label>
                    <input class="form__input" type="text" name="category_id" id="category_id" v-model="product.data.category_id">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="description">Descrição</label>
                    <textarea class="form__input" name="description" id="description" cols="30" rows="5" v-model="product.data.description"></textarea>
                    <p class="form__error"></p>
                </div>

                <hr>

                <h1>Especificação</h1>
                
                <div class="form__group">
                    <label class="form__label" for="brand">Marca</label>
                    <input class="form__input" type="text" name="brand" id="brand" v-model="product.data.brand">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="height">Altura (Produto)</label>
                    <input class="form__input" type="text" name="height" id="height" v-model="product.data.height">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="width">Largura (Produto)</label>
                    <input class="form__input" type="text" name="width" id="width" v-model="product.data.width">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="length_prod">Comprimento (Produto)</label>
                    <input class="form__input" type="text" name="length_prod" id="length_prod" v-model="product.data.length_prod">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="weight">Peso</label>
                    <input class="form__input" type="text" name="weight" id="weight" v-model="product.data.weight">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="price">Preço</label>
                    <input class="form__input" type="text" name="price" id="price" v-model="product.data.price">
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <label class="form__label" for="images">Fotos</label>
                    <input class="form__input" type="file" name="images" id="images" v-on:change="handleImages" multiple>
                    <p class="form__error"></p>
                </div>

                <div class="form__group">
                    <button class="form__button" type="submit">Atualizar</button>
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
                description: '',
                brand: '',
                height: '',
                width: '',
                length_prod: '',
                weight: '',
                price: '',
                category_id: '',
                images: {}
            }
        }
    }),
    methods: {
        async updateProduct() {

            try {

                await this.$axios.get('/sanctum/csrf-cookie')

                const address = await this.$axios.put('/api/address/'+this.address.data.id, this.address.data)
            
                this.$parent.refreshData(address)

                this.$router.push({ name: 'address'})
            } catch (e) {

            }
        }
    },
    mounted() {
        if(!(this.$route.params.id)){
            return this.$router.push({ name: 'product'})
        }

        this.$axios.get(`/api/products/${this.$route.params.id}`)
            .then(response => {

                this.product.data = response.data;
                this.loaded = true;
            })
            .catch(error => {

                console.log(error);
            }
        );
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