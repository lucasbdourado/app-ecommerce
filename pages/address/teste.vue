<template>
    <div>
        <h1>Categorias </h1>
        <div v-for="category in categories.data">
            <Category :category="category"/>
        </div>
    </div>
</template>

<script>
import Category from '~/components/Category.vue';

    export default {
    middleware: ["verified"],
    data: () => ({
        loaded: false,
        categories: {
            children: false
        }
    }),
    methods: {
        refreshData(address) {
            this.address.data = address.data;
        },
        haveChildren(children) {
            if (children != "[]") {
                this.result = false;
            }
            else {
                this.result = true;
            }
            return {
                children: this.result,
            };
        },
    },
    async mounted() {
        try {
            this.categories = await this.$axios.get("/api/address/user");
            console.log(this.categories.data);
            this.loaded = true;
        }
        catch (e) {
        }
    },
    components: { Category }
}
</script>

<style>

    li{
        display: flex;
        gap: 10px;
    }

</style>