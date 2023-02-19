<template>
    <nav class="header__navbar">
        <ul class="header__navbar-list">
            <li><nuxt-link :to="{ name: 'index' }" class="header__navbar-item">Home</nuxt-link></li>
        </ul>
        <div v-if="$auth.loggedIn">
            <div class="header__user">
                <p>Olá, <nuxt-link :to="{ name: 'profile-information' }">{{ $auth.user.first_name }}</nuxt-link></p>
                <nuxt-link :to="{ name: 'address' }">Meus endereços</nuxt-link>
                <a href="#" class="header__navbar-item" @click.prevent="logout">Logout</a>
            </div>
        </div>

        <div v-if="!$auth.loggedIn">
            <div class="header__user">
                <nuxt-link :to="{ name: 'login' }" class="header__navbar-item">Login</nuxt-link>
                <nuxt-link :to="{ name: 'register' }" class="header__navbar-item">Register</nuxt-link>
            </div>
        </div>
    </nav>
</template>

<script>
    export default {
        methods: {
            async logout () {
                await this.$auth.logout()
            }
        },
    }
</script>

<style>
    nav{
        padding: 0 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    ul, a{
        list-style: none;
        text-decoration: none;
    }

    .header__user{
        display: flex;
        align-items: center;
        gap: 10px;
    }
</style>