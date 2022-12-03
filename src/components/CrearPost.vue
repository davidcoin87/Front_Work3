<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Agergar/Editar Posts
            </div>
            <div class="card-body">

                <form v-on:submit.prevent = "addRegistro">
                    <div class="form-group">
                      <label for="nombre">Nombre:</label>
                      <input type="text"
                        class="form-control" required name="nombre" id="nombre" v-model="post.name" aria-describedby="helpId" placeholder="Escriba un Nombre">
                      <small id="helpId" class="form-text text-muted">Dato Oligatorio</small>
                    </div>

                    <div class="form-group">
                      <label for="descripcion">Descripcion:</label>
                      <input type="text"
                        class="form-control" required name="descripcion" id="descripcion" v-model="post.description" aria-describedby="helpId" placeholder="Escriba una descripción">
                      <small id="helpId" class="form-text text-muted">Dato Oligatorio</small>
                    </div>

                    <div class="form-group">
                        <label for="category_id">Categoria</label>
                        <select name="" id="" v-model="post.category_id" class="form-control">
                            <option v-for="category in categories" :key="category.id" :value="category.id">{{ category.name }}</option>
                        </select>
                    </div>

                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-success">Agregar</button>
                        <router-link :to="{name:'Listar Post'}" class="btn btn-warning">Cancelar</router-link>
                    </div>
                </form>
                
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CrearPost',
    data() {
        return{
            post:{},
            categories:[],
            active: true,
            active2: true,
            active3: false,
        }
    },
    created:function(){
        this.consultarCategorias();
        this.serchDataById();
    },
    methods: {
        addRegistro(){
            console.log(this.post);
            var datosEnvio = {
                id:this.post.id,
                name:this.post.name,
                desription:this.post.description,
                category_id:this.post.category_id,
                state:this.post.state
            }
            console.log('datos envio',datosEnvio);

            if(datosEnvio.id === undefined)
            {
                datosEnvio.state = true;
                console.log('datos envio new',datosEnvio);
                console.log('nuevo registro');
                fetch('http://127.0.0.1:8000/api/post/store', {
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
                    window.location.href='../listarPost'
                }))
            }else{
                console.log('editar registro');
                console.log('datos envio edit',datosEnvio);
                var id = this.$route.params.id;
                var url = 'http://127.0.0.1:8000/api/post/'+id+'/update';

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
                    console.log(datosRpta);
                    window.location.href='../listarPost';
                }))
                
            }
            
        },
        serchDataById() {
            var id = this.$route.params.id;
            console.log(id);
            if(id > 0){
                var url = 'http://127.0.0.1:8000/api/post/'+id+'/getById';
                console.log(url);
                fetch(url)
                .then(rta => rta.json())
                .then((datosRta) => {
                    console.log(datosRta)
                    this.post.id = datosRta.result.id;
                    this.post.name = datosRta.result.name;
                    this.post.description = datosRta.result.desription;
                    this.post.category_id = datosRta.result.category_id;
                    this.post.state = datosRta.result.state;
                    console.log(this.post);
                })
                .catch(console.log)
            }
        },
        consultarCategorias() {
            fetch('http://127.0.0.1:8000/api/category/all')
            .then(rta => rta.json())
            .then((datosRta) => {
                console.log(datosRta)
                this.categorias = [];
                if(typeof datosRta.result[0].success==='undefined')
                {
                    this.categories = datosRta.result;
                    console.log('Categorias', this.categories);
                }
            })
            .catch(console.log)
        }
    }
}
</script>