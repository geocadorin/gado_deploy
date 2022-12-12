<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'

export default {
    page: {
        title: 'Listar Dieta',
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
            title: 'Listar Dieta',
            dietas: [],
            dietasFiltradas: [],
            filtro: ''
        }
    },
    mounted() {

        if (localStorage.dietas) {
            this.dietas = JSON.parse(localStorage.dietas);
            this.dietasFiltradas = this.dietas
           
        }
    },
    methods: {
        buscar() {
            this.dietasFiltradas = this.dietas.filter((dieta) => {
                return dieta.codigo.toLowerCase().includes(this.filtro.toLowerCase()) ||
                    dieta.descricao.toLowerCase().includes(this.filtro.toLowerCase()) 
            });

            if (this.filtro == '') this.dietasFiltradas = this.dietas
           
        },

        editar(id) {
            this.$router.push({
                name: 'Cadastro de Dieta',
                query: {
                    parametros: {
                        id: id
                    }
                }
            })
        },


        deletar(id) {
            console.log('Deletou ', id)

            this.$swal.fire({
                position: 'center',
                title: 'Você tem certeza?',
                text: "Ao confirmar a dieta será deletada",
                icon: 'warning',
                showCancelButton: true,
                //confirmButtonColor: '#3085d6',
                //cancelButtonColor: '#d33',
                confirmButtonText: 'Sim',
                cancelButtonText: 'Não',
            }).then((result) => {
                if (result.isConfirmed) {
                    //remove da tabela
                    this.dietasFiltradas = this.dietas.filter((dieta) => {
                        return dieta.id != id
                    });

                    //remove do localStorage
                    this.dietas = this.dietas.filter((dieta) => {
                        return dieta.id != id
                    });

                    localStorage.dietas = JSON.stringify(this.dietas);

                    this.$swal.fire({
                        position: 'top-end',
                        title: 'Deletado',
                        text: "Dieta deletada com sucesso.",
                        icon: 'success',
                        showConfirmButton: false,
                    })
                }
            })

        }
    },
}
</script>

<template>
<Layout>
    <PageHeader :title="title" />

    <div class="row">

        <div class="col-lg-12">
            <div class="card">
                <div class="row p-3">
                    <div class="col-md-4">
                        <div class="form-group">
                            <label for="tag">
                                Filtro
                            </label>
                            <input id="filtro" v-model="filtro" name="filtro" class="form-control" type="text" placeholder="Filtre pelo Código ou descrição" />
                        </div>
                    </div>
                    <div class="col-md-4 mt-2">
                        <div class="form-group mt-3">
                            <button class="btn btn-warning" type="button" @click="buscar()">Buscar</button>
                        </div>
                    </div>

                </div>

                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table table-striped mb-0">
                            <thead>
                                <tr>
                                    <th scope="col">#Código</th>
                                    <th scope="col">Descrição</th>
                                    <th scope="col">Observação</th>
                                    <th scope="col" class="text-center">Opções</th>

                                </tr>
                            </thead>

                            <tbody>
                                <tr v-for="dieta in dietasFiltradas">
                                    <th scope="row">{{dieta.codigo}}</th>
                                    <td>{{dieta.descricao}}</td>
                                    <td>{{dieta.observacao}}</td>
                                    <td class="text-center">
                                        <b-dropdown variant="light" size="sm">
                                            <template slot="button-content">
                                                <feather type="menu"></feather>
                                            </template>
                                            <b-dropdown-item @click="editar(dieta.id)">Editar</b-dropdown-item>
                                            
                                            <b-dropdown-item @click="deletar(dieta.id)">Deletar</b-dropdown-item>
                                        </b-dropdown>
                                    </td>
                                </tr>
                                <tr v-show="dietasFiltradas.length == 0">
                                    <td colspan="6" class="text-center font-weight-bold">
                                        <h5>Nenhuma Dieta Encontrada</h5>
                                    </td>
                                </tr>

                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- end row -->
</Layout>
</template>
