<template>
        <!-- use "v-model" to bind data -->
        <!-- when country dropdown changes this will call a method called "onChange" -->
        <select 
            v-model="selected" 
            class="form-select mt-1 block w-full border p-3 rounded"
            @change="onChange()"
            >
            <option value="0">Select Country</option>
            <!-- "v-for" is a directive to loop through the "countries" to output an option -->
            <!-- "country" is an object that has an ID and the ID is called thru the API -->
            <option v-for="country in countries" :value="country.ID">
                {{ country.Country }}
            </option>
        </select>
</template>

<script>
export default {
    name: 'CountrySelect',
    props: ['countries'], 
    // data is a function
    data() {
        // returns an object
        return {
            // selected is a value
            selected: 0,
        }
    },
    methods: {
        onChange() {
            const country = this.countries.find((item) => item.ID === this.selected)
            this.$emit('get-country', country)
        }
    }
}
</script>