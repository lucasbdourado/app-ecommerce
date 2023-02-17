<template>
    <form @submit.prevent="updateProfileInformation">
      <div class="form__group">
            <label class="form__label" for="first_name">Primeiro Nome</label>
            <input class="form__input" type="text" name="first_name" id="first_name" v-model="form.first_name">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
            <label class="form__label" for="last_name">Sobrenome</label>
            <input class="form__input" type="text" name="last_name" id="last_name" v-model="form.last_name">
          <p class="form__error"></p>
        </div>
        
        <div class="form__group">
            <label class="form__label" for="email">E-Mail</label>
            <input class="form__input" type="email" name="email" id="email" v-model="form.email">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
            <label class="form__label" for="phone">Telefone</label>
            <input class="form__input" type="text" name="phone" id="phone" v-model="form.phone">
          <p class="form__error"></p>
        </div>

        <div class="form__group">
        <button class="form__button" type="submit">Atualizar</button>
        </div>
  </form>
</template>

<script>
export default {
    data() {
        return {
            form: {
                first_name: this.$auth.user.first_name,
                last_name: this.$auth.user.last_name,
                name: this.$auth.user.name,
                email: this.$auth.user.email,
                phone: this.$auth.user.phone,
            }
        }
    },
    methods: {
        async updateProfileInformation () {
            try{
                await this.$axios.get('sanctum/csrf-cookie')
                await this.$axios.put('user/profile-information', this.form)

                await this.$auth.fetchUser()
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