<template>
    <div class="card">
        <!-- <div class="card-header">
                            <h4 class="card-title">Basic Inputs</h4>
                        </div> -->

        <div class="card-body">
            <div class="row">

                <div class="col-md-12">
                    <form @submit.prevent="StoreForm()" action="/siswa" method="POST">
                        <div class="form-group">
                            <label for="basicInput">Semestre</label>
                            <input type="text" class="form-control " id="basicInput"
                                v-model="form.level" v-bind:class="{ 'is-invalid': erorr.level }">
                            <div v-if="erorr.level" class="invalid-feedback">
                                {{ erorr.level[0]  }}
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="helpInputTop">Asignaturas</label>
                            <input type="text" class="form-control" id="helpInputTop"
                                v-model="form.jurusan" v-bind:class="{ 'is-invalid': erorr.jurusan }">
                            <div v-if="erorr.jurusan" class="invalid-feedback">
                                {{ erorr.jurusan[0]  }}
                            </div>
                        </div>

                        <router-link to="/asignatura" class="btn btn-danger">Cancelar</router-link>
                        <button type="submit" class="btn btn-success">Actualizar</button>

                    </form>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props : ['id'],
        data() {
            return {
                form: {
                    level: ' ',
                    jurusan: ' ',
                },
                option: [],
                erorr: [],
                data: {}
            }
        },
        methods: {
            getData() {
                axios.get('/api/semestre/' + this.id + '/edit').then((response) => {
                    this.form = { // add data to v-model="form.*" 
                        level: response.data.level,
                        jurusan: response.data.jurusan,
                    }
                });
            },
            StoreForm() {
                console.log(this.form)
                axios.put('/api/semestre/' + this.id, this.form).then((response) => {
                    if (response.data.status) {
                        // console.log(response.data.messege);
                        this.$noty.success(response.data.messege);
                        this.$router.push({
                            name: 'semestre',
                        });
                    }
                }).catch((erorr) => {
                    // console.log(erorr.response.data.errors);
                    this.erorr = erorr.response.data.errors
                })
            }
        },
        mounted() {
            this.getData();
        },
    }

</script>
