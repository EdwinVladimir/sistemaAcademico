<template>
    <div>
        <h1 class="text-center">Reporte de Mis Notas</h1>

        <div class="my-4" style="background: white; padding: 2em;">
            
            <form @submit.prevent="descargar('preview')">
                <div class="row">
                    <div class="col-lg-3 col-md-3 col-sm-4 col-12">
                        <div class="form-group">
                            <label for="">Periodos</label><br>
                            <select v-model="periodo_id" class="form-control" @change="CargarCursos()" required>
                                <option v-for="periodo in lista_periodos" :value="periodo.id">{{periodo.ano_per +' - '+ periodo.peri_per}}</option>
                            </select>
                        </div>
                    </div>
                    <div class="col-lg-3 col-md-3 col-sm-4 col-12">
                        <div class="form-group">
                            <label for="">Cursos</label><br>
                            <select v-model="curso_id" class="form-control" @change="listarMaterias" required>
                                <option v-for='curso in lista_cursos' :value="curso.id">{{curso.nom_cur}}</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="col-lg-3 col-md-3 col-sm-4 col-12">
                        <div class="form-group">
                            <label for="" style="color: white;">Individual</label><br>
                            <button class="btn btn-primary form-control"><i class="fa fa-eye"></i> Ver Reporte</button>
                        </div>
                    </div>
                    <div class="col-lg-3 col-md-3 col-sm-4 col-12">
                        <div class="form-group">
                            <label for="" style="color: white;">Individual</label><br>
                            <button type="button" class="btn btn-primary form-control" style="color: white;" @click="descargar('descargar')"><i class="fa fa-file-pdf"></i> Descargar</button>
                        </div>
                    </div>
                </div>
            </form>

            <div class="row">
                <div class="col-lg-12">
                    <iframe id="viePDF" src="" width="100%" height="680px"></iframe>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {

    data() {

        return {

            alumno_id: 0,
            curso_id: 0,
            periodo_id: 0,

            lista_cursos: [],
            lista_periodos: [],
        };
    },

    mounted()
    {
        this.alumno_id = alumno_id;

        this.listarPeriodos();
    },

    methods: {

        async listarPeriodos()
        {
            const res = await axios.get('../reporte/listar/periodos/'+ this.alumno_id);

            let lista_cursos   = res.data.lista_cursos;
            let lista_periodos = res.data.lista_periodos;

            this.lista_cursos            = lista_cursos;
            this.lista_periodos          = lista_periodos;
        },
        
        async CargarCursos()
        {
            const res = await axios.get('../reporte/listar/cursos-alumnos/'+ this.periodo_id +'/alumno_id');

            this.lista_cursos = res.data.lista_cursos;
        },

        async descargar(type)
        {
            $('#viePDF').attr('src', '../reporte/notas/descargar/'+ this.curso_id +'/'+ this.periodo_id +'/'+ type);
            //window.open('http://127.0.0.1:8000/reporte/notas/descargar/'+ this.curso_id +'/'+ this.periodo_id);
        },
    },
};
</script>

<style>
  .mostrar{
    display: list-item;
    opacity: 1;
    background: rgba(75, 56, 143, 0.705);
  }
</style>