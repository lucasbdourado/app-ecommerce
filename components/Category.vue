<template>
    <ul>
        <li>{{ category.title }}</li>

        <div v-for="children in category.children">
            <Category :category="children"/>
        </div>
    </ul>
</template>

<script>
    export default {
        props: ['category'],
        data() {
            return {
                data: {}
            }
        }
    }
</script>

<style scoped>
</style>