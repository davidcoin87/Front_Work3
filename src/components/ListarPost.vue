<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Posts
                <a name="" id="" class="btn btn-success" href="/crearPost/0" role="button">Nuevo</a>
            </div>
            <div class="card-body">
                <table class="table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Categoria</th>
                            <th>Nombre Post</th>
                            <th>Descripción</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="post  in posts" :key="post.id">
                            <td>{{post.id}}</td>
                            <td>{{post.categoryName}}</td>
                            <td>{{post.name}}</td>
                            <td>{{post.description}}</td>
                            <td>

                                <div class="btn-group" role="group" aria-label="">
                                    <button type="button" v-on:click="editPost(post)" class="btn btn-info">Editar</button>
                                    <!-- <router-link :to="{name:'Crear', param:{id:category.id}}" class="btn btn-info">Editar</router-link> -->
                                    <button type="button" v-on:click="deletePost(post.id)" class="btn btn-danger">Borrar</button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ListarPost',
    data() {
        return{
            posts:[]
        }
    },
    created:function() {
        this.consultarPosts();
    },
    methods: {
        consultarPosts() {
            fetch('http://127.0.0.1:8000/api/post/all')
            .then(rta => rta.json())
            .then((datosRta) => {
                console.log(datosRta)
                this.posts = [];
                if(typeof datosRta.result[0].success==='undefined')
                {
                    this.posts = datosRta.result;
                }
            })
            .catch(console.log) 
        },
        deletePost(id) {

            const Swal = require('sweetalert2');

            Swal.fire({
                title: 'Delete',
                text: 'Esta seguro de borrar este registro?',
                icon: 'question',
                showCancelButton: true,
                confirmButtonColor: '#1f9bcf',
                cancelButtonColor: '#d9534f',
                cancelButtonText: 'No',
                confirmButtonText: 'Yes'
            }).then((result) => {
                if(result.isConfirmed){
                    console.log(id);
                    var url = 'http://127.0.0.1:8000/api/post/'+id+'/destroy';
                    fetch(url,{
                        method: "DELETE"
                    })
                    .then(rta => rta.json())
                    .then((datosRta) => {
                        console.log(datosRta)
                        window.location.href="listarPost";
                        Swal.fire(
                            'Borrado!',
                            'El archivo fue borrado con éxito.',
                            'success'
                        )
                    })
                    .catch(console.log)
                }
            })
        },
        editPost(obj) {
            window.location.href="crearPost/"+obj.id;
        }
    },

}
</script>