<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Categorias
                <a name="" id="" class="btn btn-success" href="/crear/0" role="button">Nuevo</a>
            </div>
            <div class="card-body">
                <table class="table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Descripción</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="category  in categorias" :key="category.id">
                            <td>{{category.id}}</td>
                            <td>{{category.name}}</td>
                            <td>{{category.description}}</td>
                            <td>

                                <div class="btn-group" role="group" aria-label="">
                                    <button type="button" v-on:click="editCategory(category)" class="btn btn-info">Editar</button>
                                    <!-- <router-link :to="{name:'Crear', param:{id:category.id}}" class="btn btn-info">Editar</router-link> -->
                                    <button type="button" v-on:click="deleteCategory(category.id)" class="btn btn-danger">Borrar</button>
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

    name: 'ListarView',
    props: {
        msg: String
    },
    data() {
        return{
            categorias:[]
        }
    },

    created:function() {
        this.consultarCategorias();
    },
    methods: {
        consultarCategorias() {

            fetch('http://127.0.0.1:8000/api/category/all')
            .then(rta => rta.json())
            .then((datosRta) => {
                console.log(datosRta)
                this.categorias = [];
                if(typeof datosRta.result[0].success==='undefined')
                {
                    this.categorias = datosRta.result;
                }
            })
            .catch(console.log)            
        },
        deleteCategory(id) {

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
                    var url = 'http://127.0.0.1:8000/api/category/'+id+'/destroy';
                    fetch(url,{
                        method: "DELETE"
                    })
                    .then(rta => rta.json())
                    .then((datosRta) => {
                        console.log(datosRta)
                        window.location.href="listar";
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
        editCategory(obj) {
            window.location.href="crear/"+obj.id;
        }
    }
}
</script>