<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'
//import router from '@/router'



export default {
    page: {
        title: 'Visualizar Animal',
        meta: [{
            name: 'description',
            content: appConfig.description
        }],
    },
    components: {
        Layout,
        PageHeader
    },
    data() {
        return {
            title: 'Visualizar Animal',
            animais: [],
            animalData:{}

           
        }
    },
    mounted() {

        if (localStorage.animais) {
            this.animais = JSON.parse(localStorage.animais);

        }

        if (this.$router.currentRoute.query.parametros && this.$router.currentRoute.query.parametros.id) {
            this.animalData.id = this.$router.currentRoute.query.parametros.id

            this.buscarPorId()
        }
    },
    
    methods: {

        buscarPorId() {
            const animalSelecionado = this.animais.filter((animal) => {
                return animal.id == this.animalData.id
            });
            this.preencherForm(animalSelecionado[0])
        },

        preencherForm(animal) {
            this.animalData.tag = animal.tag
            this.animalData.idade = animal.idade
            this.animalData.peso = animal.peso
            this.animalData.raca = animal.raca
            this.animalData.sexo = animal.sexo
            this.animalData.dieta = animal.dieta
            this.animalData.faseVida = animal.faseVida
        },

        

        
    },
}
</script>

<template>
<Layout>
    <PageHeader :title="title" />
    <div class="row">
        <div class="col-lg-12">
            <div class="card">
                <div class="card-body">
                    <h2>Visualizar</h2>

                    
                </div>
            </div>
            <!-- end card--->
        </div>
        <!-- end col -->
    </div>
    <!-- end row -->

</Layout>
</template>
