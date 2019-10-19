<template>
  <div id="app"> 

  <h1> Laboratorio 4 - Frontend </h1>

  <p>Nombre: {{ name }}</p>
  <input v-model="name"> <br/>
  <p>Creditos: {{ credits }}</p>
  <input type="number" v-model="credits"> <br/>
  <p>Profesor: {{ professor }}</p>
  <input v-model="professor"> <br/>

  <button v-on:click= "consumeREST"> Crear con REST </button>
  <button v-on:click= "consumeGraphQL"> Crear con GraphQL </button> <br/>

  <div v-if = "message != null"> <APIResponse v-bind:data = "message"  /> </div>
  

  </div>
</template>

<script>
import APIResponse from './components/APIResponse.vue'
import axios from 'axios'

export default {
    name: 'app',
    components: {
        APIResponse
  
    },

    data: ()=>({
        message: null,
        name: "Nombre del curso",
        professor: "Nombre del profesor",
        credits: 4
        }),
    methods:   {
        consumeREST : async function () {
            
            const url = "http://3.229.229.156:3000/courses-ms/resources/courses"  //Url del microservicio courses-ms
            const response = await axios.post(url,
            {
                
                    name: this.name,
                    credits: this.credits,
                    professor: this.professor
                    
                }            
            )
            .catch(e => console.log('Error: ', e) )


            console.log(response)
            if(response && response.status == 201) {
                    this.message = {
                        msg:"Curso creado satisfactoriamente desde Microservicio:",                   
                        code: response.data.code,
                        name: this.name,
                        credits: this.credits,
                        profesor: this.professor
                     }

                }

        },
        consumeGraphQL: async function() {
        
            const url = "http://3.229.229.156:5000/graphql/" //URL del Api-Gateway
            const response = await axios.post(url, {
                "query" : `mutation {
                        createCourse(course: {
                            name: "${this.name}"
                            credits: ${this.credits}
                            professor: "${this.professor}"
                        })
                    {
                           code
                           name
                           credits
                           professor
                    }
                }`          
            }).catch(e => console.log('Error: ', e) )

            console.log(response)
            if( response && response.status == 200) {
                    
               this.message = {
                        msg:"Curso creado satisfactoriamente desde Microservicio:",                   
                        code: response.data.data.createCourse.code,
                        name: response.data.data.createCourse.name,
                        credits: response.data.data.createCourse.credits,
                        professor: response.data.data.createCourse.professor
                }
            }
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

button{

    margin:2rem;
    padding:1rem;
    background-color: greenyellow;
    border:0px ;
}
button:hover{
    background-color: chartreuse;
    cursor: pointer; 

}

</style>

