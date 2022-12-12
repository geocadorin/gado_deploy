<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'
//import router from '@/router'

import {
    required,
    email,
    minLength,
    sameAs,
    maxLength,
    minValue,
    maxValue,
    numeric,
    url,
    alphaNum,
} from 'vuelidate/lib/validators'

export default {
    page: {
        title: 'Cadastro de Dieta',
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
            title: 'Cadastro de Dieta',
            dietas: [],
            form: {
                id: '',
                codigo: '',
                descricao: '',
                observacao: '',
                insumos: []

            },
            optionsInsumo: [
                '',
                'Pasto',
                'Farelo de Milho',
                'Sal',
                'Ração'
            ]

        }
    },
    mounted() {
        if (localStorage.dietas) {
            this.dietas = JSON.parse(localStorage.dietas);
            
        }

        if (this.$router.currentRoute.query.parametros && this.$router.currentRoute.query.parametros.id) {
            this.form.id = this.$router.currentRoute.query.parametros.id

            const dietaSelecionada = this.dietas.filter((dieta) => {
                return dieta.id == this.form.id
            });

            console.log('Dietaaa ', dietaSelecionada)

            const dieta = dietaSelecionada[0]
            this.form.descricao = dieta.descricao
            this.form.codigo = dieta.codigo
            this.form.observacao = dieta.observacao
            this.form.insumos = dieta.insumos
        }
    },
    validations: {

    },
    methods: {
        buscarPorId() {
            const dietaSelecionada = this.dietas.filter((dieta) => {
                return dieta.id == this.form.id
            });

            
        },

        preencherForm(dieta) {
            console.log('DIETA',dieta)
            this.form.descricao = dieta.descricao
            this.form.codigo = dieta.codigo
            this.form.observacao = dieta.observacao
            this.form.insumos = dieta.insumos
        },

        removerInsumo(id){
            this.form.insumos = this.form.insumos.filter((item) => {
                return item.id != id
            });
        },

        addInsumo() {
            const myId = this.generateUUID()
            this.form.insumos.push({
                id:myId,
                insumo: '',
                gramas: '',
                dias: ''
            })
        },

        buscarPorId() {

        },

        preencherForm(animal) {

        },

        generateUUID() { // Public Domain/MIT
            var d = new Date().getTime(); //Timestamp
            var d2 = ((typeof performance !== 'undefined') && performance.now && (performance.now() * 1000)) || 0; //Time in microseconds since page-load or 0 if unsupported
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16; //random number between 0 and 16
                if (d > 0) { //Use timestamp until depleted
                    r = (d + r) % 16 | 0;
                    d = Math.floor(d / 16);
                } else { //Use microseconds since page-load if supported
                    r = (d2 + r) % 16 | 0;
                    d2 = Math.floor(d2 / 16);
                }
                return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
            });
        },
        handleSubmit(e) {
            if (this.form.id == '') {
                    //Cria uuid
                    this.form.id = this.generateUUID()

                    //Cria obj animal e adiciona ao arr
                    this.dietas.push({
                        id: this.form.id,
                        codigo:this.form.codigo,
                        descricao:this.form.descricao,
                        observacao: this.form.observacao,
                        insumos: this.form.insumos
                        
                    })

                    //Salva no localStorage como String(sempre transformar em string para salvar)
                    localStorage.dietas = JSON.stringify(this.dietas);

                    //Direciona para a ppágina de listagem
                    this.$router.push({
                        name: 'Listar Dieta'
                    })

                    //Informa o usuario
                    this.$swal.fire({
                        position: 'top-end',
                        icon: 'success',
                        //title: 'Sucesso',
                        text: 'Cadastro salvo com sucesso',
                        showConfirmButton: false,
                    })
                } else {
                    this.dietas = this.dietas.filter((dieta) => {
                        return dieta.id != this.form.id
                    });

                    this.dietas.push({
                        id: this.form.id,
                        codigo:this.form.codigo,
                        descricao:this.form.descricao,
                        observacao: this.form.observacao,
                        insumos: this.form.insumos
                    })

                    //Salva no localStorage como String(sempre transformar em string para salvar)
                    localStorage.dietas = JSON.stringify(this.dietas);

                    //Direciona para a ppágina de listagem
                    this.$router.push({
                        name: 'Listar Dieta'
                    })

                    //Informa o usuario
                    this.$swal.fire({
                        position: 'top-end',
                        icon: 'success',
                        //title: 'Sucesso',
                        text: 'Cadastro atualizado com sucesso',
                        showConfirmButton: false,
                    })
                }
        },

        validateFields() {

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
                <div class="card-body">

                    <p class="sub-header font-weight-bold">Os campos com (<span class="text-danger">*</span>) são obrigatórios</p>
                    <form @submit.prevent="handleSubmit">
                        <div class="row">
                            <div class="col-md-6">
                                <div class="form-group">
                                    <label for="tag">
                                        Código
                                        <span class="text-danger">*</span>
                                    </label>
                                    <input id="codigo" v-model="form.codigo" name="codigo" class="form-control"  type="text" placeholder="Insira o código da dieta" />
                                    
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="form-group">
                                    <label for="tag">
                                        Descrição
                                        <span class="text-danger">*</span>
                                    </label>
                                    <input id="descricao" v-model="form.descricao" name="descricao" class="form-control"  type="text" placeholder="Insira a descrição da dieta" />
                                    
                                </div>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col-md-12">
                                <div class="form-group">
                                    <label for="tag">
                                        Observação
                                    </label>
                                    <textarea id="observacao" v-model="form.observacao" name="observacao" class="form-control" type="text" rows="4"></textarea>
                                </div>
                            </div>
                        </div>
                        <div class="pt-3 mt-3" style="display: inline !important; margin-top: 16px !important;">
                            <h4 style="display: inline !important; ">Insumos</h4>
                            <button @click="addInsumo" class="btn btn-info btn-xs float-right" type="button">+ Adicionar Insumo</button>
                        </div>
                        <hr>
                        <div class="row" v-for="(item, i) in form.insumos">
                            <template>
                                <div class="col-md-4">
                                    <div class="form-group">
                                        <label for="insumo">
                                            Insumo
                                            <span class="text-danger">*</span>
                                        </label>
                                        <select v-model="form.insumos[i].insumo" :id='"insumo"+[i]' :name='"insumo"+[i]' class="form-control custom-select" type="text">
                                            <option v-for="insumo in optionsInsumo">{{insumo}}</option>
                                        </select>

                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="form-group">
                                        <label for="gramas">
                                            Quantidade (gramas)/dia
                                            <span class="text-danger">*</span>
                                        </label>
                                        <input v-model="form.insumos[i].gramas" :id='"gramas"+[i]' :name='"gramas"+[i]' class="form-control" type="text" />

                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="form-group">
                                        <label for="dias">
                                            Quantidade de dias
                                            <span class="text-danger">*</span>
                                        </label>
                                        <input v-model="form.insumos[i].dias" :id='"dias"+[i]' :name='"dias"+[i]' class="form-control" type="text" />
                                    </div>
                                </div>
                                <div class="col-md-2 " style="margin-top: 1.6rem;">
                                    <div class="form-group ">
                                        <label for="btn">
                                            <button @click="removerInsumo(form.insumos[i].id)" class="btn btn-outline-danger" type="button">Excluir</button>
                                        </label>

                                    </div>
                                </div>
                            </template>

                        </div>
                        <hr class="mt-3" v-show="(form.insumos.length > 0)">

                        <div class="form-group text-right m-b-0">
                            <button class="btn btn-primary" type="submit">Salvar</button>
                        </div>
                    </form>
                </div>
            </div>
            <!-- end card--->
        </div>
        <!-- end col -->
    </div>
    <!-- end row -->

</Layout>
</template>

<style scoped>

</style>
