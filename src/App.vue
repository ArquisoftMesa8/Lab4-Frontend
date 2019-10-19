<template>
  <div id="app">
  
  
  <p>{{ message }}</p>
 
  <p>Name: {{ name }}</p>
 
  <input v-model="name">
  <br/>
  <p>Credits: {{ credits }}</p>
  <input type="number" v-model="credits">
  <br/>
  <button v-on:click= "consumeREST"> Crear con REST </button>
  <button v-on:click= "consumeGraphQL"> Crear con GraphQL </button>


  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'app',
    components: {
  
    },

    data: ()=>({
        message: '',
        name: "Default Name",
        credits: 0
        }),
    methods:   {
        consumeREST : async function () {


            this.message = "Rest"
            const url = "http://3.229.229.156:3000/courses-ms/resources/courses/"

            const response = await axios.post(url,
                {
                    headers:{
                        'Access-Control-Allow-Origin': '*'
                    },
                    name: this.name,
                    credits: this.credits
                }
            
            )
            .catch(e => console.log('Error: ', e) )

            //console.log(response)

        },
        consumeGraphQL: async function() {
            
            
            this.message = "GraphQL"
            const url = "http://3.229.229.156:5000/graphql/"

            const response = await axios.post(url, {
                "query" : `mutation {
                        createCourse(course: {
                            name: "${this.name}"
                            credits: ${this.credits}
                        })
                    {
                            name
                            credits
                    }
                }`          
            }).catch(e => console.log('Error: ', e) )

            //console.log(response)

        }                
    }
}   

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
