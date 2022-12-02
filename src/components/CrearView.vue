<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Agergar/Editar Categorias
            </div>
            <div class="card-body">

                <form v-on:submit.prevent = "addRegistro">
                    <div class="form-group">
                      <label for="nombre">Nombre:</label>
                      <input type="text"
                        class="form-control" required name="nombre" id="nombre" v-model="categoria.name" aria-describedby="helpId" placeholder="Escriba un Nombre">
                      <small id="helpId" class="form-text text-muted">Dato Oligatorio</small>
                    </div>

                    <div class="form-group">
                      <label for="descripcion">Descripcion:</label>
                      <input type="text"
                        class="form-control" required name="descripcion" id="descripcion" v-model="categoria.description" aria-describedby="helpId" placeholder="Escriba una descripción">
                      <small id="helpId" class="form-text text-muted">Dato Oligatorio</small>
                    </div>

                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-success">Agregar</button>
                        <router-link :to="{name:'Listar'}" class="btn btn-warning">Cancelar</router-link>
                    </div>
                </form>
                
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return{
            categoria:{

            }
        }
    },
    created:function(){
        this.serchDataById();
    },
    methods: {
        addRegistro(){
            console.log(this.categoria);
            var datosEnvio = {
                id:this.categoria.id,
                name:this.categoria.name,
                description:this.categoria.description
            }
            console.log(datosEnvio);

            if(datosEnvio.id === undefined)
            {
                console.log('nuevo registro');
                fetch('http://127.0.0.1:8000/api/category/store', {
                    method:"POST",
                    body:JSON.stringify(datosEnvio),
                    headers: {
                        "Content-type": "application/json",
                    }
                })
                .then(rpta => {
                    rpta.json(),
                    console.log(rpta)
                })
                .then((datosRpta => {
                    console.log(datosRpta);
                    //window.location.href='../listar'
                }))
            }else{
                console.log('editar registro');
                var id = this.$route.params.id;
                var url = 'http://127.0.0.1:8000/api/category/'+id+'/update';

                fetch(url, {
                    method:"PUT",
                    body:JSON.stringify(datosEnvio),
                    headers: {
                        "Content-type": "application/json",
                    }
                })
                .then(rpta => {
                    rpta.json(),
                    console.log(rpta.result)
                })
                .then((datosRpta => {
                    console.log(datosRpta.result);
                    window.location.href='../listar'
                }))
                
            }
            
        },
        serchDataById() {
            var id = this.$route.params.id;
            console.log(id);
            if(id > 0){
                var url = 'http://127.0.0.1:8000/api/category/'+id+'/getById';
                console.log(url);
                fetch(url)
                .then(rta => rta.json())
                .then((datosRta) => {
                    console.log(datosRta)
                    this.categoria.id = datosRta.result.id;
                    this.categoria.name = datosRta.result.name;
                    this.categoria.description = datosRta.result.description;
                    console.log(this.categoria);
                })
                .catch(console.log)
            }
        }


    },
}
</script>