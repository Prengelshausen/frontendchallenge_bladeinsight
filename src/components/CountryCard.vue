<template>
    <div class="grid xl:grid-cols-4 lg:grid-cols-3 md:grid-cols-2 gap-4 mx-auto place-items-center">
        <router-link v-for="c of this.filtered" :key="c.cca2" :to="`/detail/${c.cca2}`" 
            class="flex flex-col border rounded-md my-4 hover:shadow-lg w-9/12 h-92">
            <div>
                <img class="w-full h-44 object-cover object-center rounded-t-md" :src="c.flags.svg" :alt="c.flags.alt">
                <div class="px-4">
                    <p class="font-bold mt-4"> {{ c.name.common }} </p>
                    <div class="my-4 text-left">
                        <p><b>Population:</b> {{ c.population.toLocaleString("pt-PT") }}</p>
                        <p><b>Capital:</b> {{showCapital(c)}}</p>
                        <p><b>Region:</b> {{ c.region }}</p>
                    </div>
                </div>
            </div>
        </router-link>
    </div>
</template>

<script>
    export default {
        props: {
            filtered: {
                type: Array
            },
        },
        methods: {
            showCapital(country) {
                if (!country.capital) return "No Capital specified";
                return country.capital[0];
            }
        },
    }
</script>

<style scoped>

</style>