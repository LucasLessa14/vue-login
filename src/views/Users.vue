<template>
    <div>
        <h1>Painel ADM</h1>

    <div class="columns is-centered">
        <div class="column is-half">
            <table class="table">
                <thead>
                    <tr>
                        <th>Nome</th>
                        <th>E-mail</th>
                        <th>Cargo</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in users" :key="user.id">
                        <th>{{ user.name }}</th>
                        <th>{{ user.email }}</th>
                        <th>{{ user.role | processRole }}</th>
                        <th>
                            <router-link :to="{name: 'Edit', params: {id: user.id}}">
                                <button class="button is-warning">Editar</button>
                            </router-link>
                            <button class="button is-danger" @click="showModalUser( user.id )">Deleter</button>
                        </th>
                    </tr>
                </tbody>
            </table>

            <div :class="{ modal: true, 'is-active': showModal }">
                <div class="modal-background"></div>
                    <div class="modal-content">
                        
                        <div class="card">
                            <header class="card-header">
                                <p class="card-header-title">Você quer realmente deletar esses usuário?</p>
                            </header>
                            <div class="card-content">
                                <div class="content">
                                Essa operação é irreverssível
                                </div>
                            </div>
                            <footer class="card-footer">
                                <a href="#" class="card-footer-item" @click="hideModal">Cancelar</a>
                                <a href="#" class="card-footer-item" @click="deleteUser">Quero deletar</a>
                            </footer>
                        </div>

                    </div>
                <button class="modal-close is-large" aria-label="close" @click="hideModal"></button>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    created() {

        var req = {
            headers: {
                Authorization: 'Bearer ' + localStorage.getItem('token')
            }
        }

        axios.get('http://localhost:8086/user', req).then(res => {
            console.log(res);

            this.users = res.data;
        }).catch(err => {
            console.log(err);
        });
    },
    data() {
        return {
            users: [],
            showModal: false,
            deleteUserId: -1
        }
    },
    methods: {
        hideModal() {
            this.showModal = false;
        },
        showModalUser(id) {
            this.deleteUserId = id;
            this.showModal = true;
        },
        deleteUser() {

            var req = {
                headers: {
                    Authorization: 'Bearer ' + localStorage.getItem('token')
                }
            }

            axios.delete('http://localhost:8086/user/'+this.deleteUserId, req).then(res => {
                console.log(res);
                this.showModal = false;
                this.users = this.users.filter(u => u.id != this.deleteUserId);
            }).catch(err => {
                console.log(err);
                this.showModal = false;
            });
        }
    },
    filters: {
        processRole: function(value) {

            if (value == 0) 
                return 'user'
            
            if (value == 1) 
                return 'admin'

            return 'bugado'
        }
    }
}
</script>

<style scoped>

</style>