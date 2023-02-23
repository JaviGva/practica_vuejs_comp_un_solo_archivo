<template>
    <div class="row container mx-auto">
        <input type="text" id="buscador" class="form-control">

        <div class="alert alert-danger mt-5" v-if="alerta" role="alert">
            El usuario no existe
        </div>

        <div v-else class="row mt-5">
            <div class="card col-xl-3 col-lg-5 col-md-7 col-sm-9 col-10 mx-auto" v-if="mostrar">
                <img :src="usuario.avatar_url" class="card-img-top" alt="...">
                <div class="card-body row">
                    <h5 class="card-title text-center">{{usuario.login}}</h5>
                    <button @click="obtenerRepositorios()" class="btn btn-primary mx-auto">Repositorios</button>
                    <a :href="'https://github.com/' + usuario.login" target="_blank" class="btn btn-light mx-auto mt-2">Url GitHub</a>

                </div>
            </div>
        </div>
    </div>
</template>

<script>

// TODO: Importar componente GitHubRepos

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            alerta: false,
            usuario: null,
            mostrar:false
        }
    },
    methods: {
        obtenerUsuario: function() {
        var url = 'https://api.github.com/users/';

        // Datos de autenticación
        /*var userAuth = process.env.VUE_APP_USERNAME || "JaviGva";
        var passAuth = process.env.VUE_APP_USERTOKEN || "Javi27520";

        var authString = userAuth + ':' + passAuth;
        var headers = new Headers();
        headers.append('Authorization', 'Basic ' + btoa(authString));
        headers.append('Content-Type', 'application/json');

        // Opciones de Fetch
        var fetchOptions = {
            method: 'GET',
            headers: headers,
            mode: 'cors',
            credentials: 'include'
        };*/


        fetch(url+document.getElementById("buscador").value)
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then(data => {
                console.log(data)
                this.usuario= data
                this.alerta = false
                this.mostrar = true
            })
            .catch(error => {
                this.alerta=true
                console.error('There was an error:', error)
            });
        },
        obtenerRepositorios: function() {
            // TODO: Función para obtener los repositorios del usuario desde la API de GítHub
            console.log("hola")
        }
    },
    mounted() {
        var inputBuscador = document.getElementById("buscador")
        inputBuscador.addEventListener('keyup', this.obtenerUsuario)
    }
}




</script>

<style scoped>
</style>
