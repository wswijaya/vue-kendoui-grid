<template>
  <div class="hello">
    <h1>Search</h1>
    <input v-model="term" type="search"> <button @click="search">Search</button><br/>
    <div v-if="results">
      <kendo-datasource ref="sharedDataSource" :data="localData" :schema-model-fields="schemaModelFields">
      </kendo-datasource>
      <kendo-grid :data-source-ref="'sharedDataSource'" :sortable="true" :filterable="true">
            <kendo-grid-column field="API" title="API"></kendo-grid-column>
            <kendo-grid-column field="Description" title="Description"></kendo-grid-column>
            <kendo-grid-column field="Auth" title="Auth"></kendo-grid-column>
            <kendo-grid-column field="HTTPS" title="HTTPS"></kendo-grid-column>
            <kendo-grid-column field="Cors" title="CORS"></kendo-grid-column>
            <kendo-grid-column field="Link" title="Link"></kendo-grid-column>
            <kendo-grid-column field="Category" title="Category"></kendo-grid-column>
        </kendo-grid>
    </div>


  </div>
</template>
<script>
  export default {
    name: 'Search',
    data: function() {
      return {
        term:'',
        localData: [],
        schemaModelFields: {
          API: { type: "string", title: "API" },
          Description: { type: "string", title: "Description" },
          Auth: {type: "string", title: "Auth"},
          HTTPS: {type: "string", title: "HTTPS" },
          Cors: {type: "string", title: "CORS"},
          Link: {type: "string", title: "Link"},
          Category: {type: "string", title: "Category"}
        },
        results:null
      }
    },
    methods: {
      search() {
        if(this.term.trim() === '') return;
        fetch(`https://api.publicapis.org/entries?title=${encodeURIComponent(this.term)}`)
          .then(res => res.json())
          .then(res => {
            this.results = res.entries;
            this.localData = res.entries;
          });
      }      
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>