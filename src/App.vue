<template>
  <div id="app">
  
  
  
 
  <p>Name: {{ name }}</p>
 
  <input v-model="name">
  <br/>
  <p>Credits: {{ credits }}</p>
  <input type="number" v-model="credits">
  <br/>
  <p>Professor: {{ professor }}</p>
  <input v-model="professor">
  <br/>
  <button v-on:click= "consumeREST"> Crear con REST </button>
  <button v-on:click= "consumeGraphQL"> Crear con GraphQL </button>
    <br/>
    
    {{ message }}

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
        name: "Nombre del curso",
        professor: "Nombre del profesor",
        credits: 4
        }),
    methods:   {
        consumeREST : async function () {
            
            const url = "http://3.229.229.156:3000/courses-ms/resources/courses"
            const response = await axios.post(url,
            {
                data:
                    {
                        name: this.name,
                        credits: this.credits,
                        professor: this.professor
                    }
                }            
            )
            .catch(e => console.log('Error: ', e) )


            console.log(response)
            if(response && response.status == 201) {
                    
                    this.message = `Curso creado satisfactoriamente desde Microservicio:\n
                    
                     Código: ${response.data.code}
                     \nNombre: ${this.name},
                     \nCréditos: ${this.credits}
                     \nProfesor: ${this.professor}
                     "`

                }

        },
        consumeGraphQL: async function() {
            
            
            this.message = "GraphQL"
            const url = "http://3.229.229.156:5000/graphql/"

            const response = await axios.post(url, {
                "query" : `mutation {
                        createCourse(course: {
                            name: "${this.name}"
                            credits: ${this.credits}
                            professor:  "${this.professor}"
                        })
                    {
                            name
                            credits
                            professor
                    }
                }`          
            }).catch(e => console.log('Error: ', e) )

            console.log(response)
                    if( response && response.status == 200) {
                    
                    this.message = `Curso creado satisfactoriamente desde API Gateway:\n
                    
                     Código: ${response.data.code}
                     \nNombre: ${this.name},
                     \nCréditos: ${this.credits}
                     \nProfesor: ${this.professor}
                     "`

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

