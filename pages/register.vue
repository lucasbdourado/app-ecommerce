<template>
  <form @submit.prevent="register">
        <div class="form__group">
            <label class="form__label" for="name">Nome</label>
            <input class="form__input" type="text" name="name" id="name" v-model="form.name">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
            <label class="form__label" for="email">E-Mail</label>
            <input class="form__input" type="email" name="email" id="email" v-model="form.email">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
          <label class="form__label" for="password">Senha</label>
          <input class="form__input" type="password" name="password" id="password" v-model="form.password">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
          <label class="form__label" for="password_confirmation">Confirme a senha</label>
          <input class="form__input" type="password" name="password_confirmation" id="password_confirmation" v-model="form.password_confirmation">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
        <button class="form__button" type="submit">Registrar</button>
        </div>
  </form>
</template>

<script>
export default {
    data() {
        return {
            form: {
                name: '',
                email: '',
                password: '',
                password_confirmation: '',
            }
        }
    },
    methods: {
        async register () {
            try{
                console.log('registrando...')
                await this.$axios.get('/sanctum/csrf-cookie')
                await this.$axios.post('register', this.form)

                await this.$auth.loginWith('laravelSanctum',{
                    email: this.form.email, password: this.form.password
                })
            }catch (e){

            }
        }
    }
}
</script>

<style>
  .form__group{
    margin-top: 10px;
  }

  .form__label{
    display: block;
    margin-bottom: 1px;
  }

  .form__input{
    padding: 8px 12px;
    font-size: 12px;
    font-weight: 700;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .form__button{
    padding: 8px 12px;
    font-size: 12px;
    font-weight: 700;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .form__error{
    color: red;
    font-size: 14px;
  }
</style>