<template>
    <form @submit.prevent="login">
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
        <button class="form__button" type="submit">Login</button>
        </div>
  </form>
</template>

<script>
export default {
    data() {
        return {
            form: {
                email: '',
                password: '',
            }
        }
    },
    methods: {
        async login () {
            try{
                await this.$auth.loginWith('laravelSanctum',{
                    data: this.form
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