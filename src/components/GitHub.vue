<template>
    <div>
        <!-- TODO: Crear componente GitHub -->
        <div class="container">
            <input type="text" class="form-control mb-4" v-model="user" @keydown.enter="obtenerUsuario" :disabled="desInput">
            <div v-if="userValid">
                <div class="row centrar">
                    <div class="col-4">
                        <div class="card" >
                            <img alt="Foto de perfil del usuario" :src="userData.avatar_url">
                            <div class="card-body">
                                <h5 class="card-title mb-3">{{userData.login}}</h5>
                                <button href="#" class="btn btn-primary" @click="obtenerRepositorios">Repositorios</button>
                                <a :href="userData.html_url" class="enlace" target="_blank">Url de GitHub</a>
                            </div>
                        </div>
                    </div>
                    
                    <div v-if="mosRepo" class="col-8">
                        <GitHubRepos :repolist="listRepos"></GitHubRepos>
                    </div>
                </div>
            </div>
            <div v-if="errorUser" class="alert alert-danger">
                El usuario que buscas en GitHub no existe
            </div>
        </div>
        
    </div>
</template>

<script>

// TODO: Importar componente GitHubRepos

import GitHubRepos from './GitHubRepos.vue';
import { encode } from "base-64";

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
        GitHubRepos,
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            user: '',
            userData: {},
            userValid: false,
            errorUser: false,
            listRepos: [],
            desInput: false,
            mosRepo: false,
        }
    },
    methods: {
        obtenerUsuario: function() {
            // TODO: Función para obtener los datos de usuario de la API de GitHub

            // TODO: Añadir lógica para resetear los cambios en el interfaz: desactivar campo de envío,
            // resetear mensaje de error, mostrar lista de repositorios,...

            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();

            headers.append('Authorization', 'Basic ' + encode(userAuth + ":" + passAuth));

            this.desInput = true;
            this.mosRepo = false;

            var url = 'https://api.github.com/users/'+this.user;

            fetch(url, {
                method: "GET",
                headers: headers,
            })
            .then(response => response.json())
            .then(data => {
                //console.log(data.message);
                if(data.message != 'Not Found'){
                    this.userData = data;
                    this.userValid = true;
                    this.errorUser = false;
                }else{
                    this.userValid = false;
                    this.errorUser = true;
                }  
            })
            //.catch(error => {
                //console.log(error);
            //})
            .finally(()=>{
                this.desInput = false;
            });

            
            //this.desInput = false;

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub


            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch

        },
        obtenerRepositorios: function() {
            // TODO: Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario

            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();

            headers.append('Authorization', 'Basic ' + encode(userAuth + ":" + passAuth));

            var url = 'https://api.github.com/users/'+this.user+'/repos';

            fetch(url, {
                method: "GET",
                headers: headers,
            })
            .then(response => response.json())
            .then(data => {
                this.listRepos = data;
                //console.log(this.listRepos);
                this.mosRepo = true;
            })
            //.catch(error => {
                //console.log(error);
            //});
           

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub



            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch

        }
    }
}
</script>

<style scoped>
.form-control:focus{
  border-color: blue;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px #0000ff;
}

.centrar{
    justify-content: center;
     align-self: center;
}

.enlace{
    text-decoration: none;
    margin-left: 10px;
    font-weight: bold;
}

</style>
