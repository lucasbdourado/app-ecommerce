<template>
    <div class="modal">
        <div class="modal__container">
            <form @submit.prevent="createProduct" class="modal__form" enctype="multipart/form-data">
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
        handleImages(event) {
            this.product.data.images = event.target.files

            console.log(this.images)
        },
        async createProduct() {

            try {

                await this.$axios.get('/sanctum/csrf-cookie')

                const formData = new FormData()
                formData.append('name', this.product.data.name)
                formData.append('description', this.product.data.description)
                formData.append('brand', this.product.data.brand)
                formData.append('height', this.product.data.height)
                formData.append('width', this.product.data.width)
                formData.append('length_prod', this.product.data.length_prod)
                formData.append('weight', this.product.data.weight)
                formData.append('price', this.product.data.price)
                formData.append('category_id', this.product.data.category_id)
                for (let i = 0; i < this.product.data.images.length; i++) {
                    console.log(i)
                    console.log(this.product.data.images[i])
                    formData.append('images[]', this.product.data.images[i])
                }
        

                const product = await this.$axios.post('/api/products', formData, {
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    },
                    processData: false,
                    contentType: false
                })
                
                this.$parent.refreshData(product)

                this.$router.push({ name: 'product'})
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