 <template>
    <div class="mt-10 mb-4">Select Country</div>
    <select
    v-model="selected"
    class="form-select  block w-full border p-3 rounded"
    @change="onChange()"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID">
      {{ country.Country }}
    </option>
  </select>
 </template>


<script>
export default {
    name: "DataBoxes",
    props: ['countries'],
    methods: {
        onChange() {
            const country = this.countries.find(item => item.ID=== this.selected)
           this.$emit('get-country',country) 
        }
    }

}
</script>

