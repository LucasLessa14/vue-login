<template>
    <div>
        <h1>Edição de usuário</h1>
        <hr>

        <div class="columns is-centered">
            <div class="column is-half">
                <div v-if="error != undefined">
                    <div class="notification is-danger">
                        <p>{{ error }}</p>
                    </div>
                </div>

                <p>Nome</p>
                <input type="text" placeholder="Digite seu nome" class="input" v-model="name">
                <p>E-mail</p>
                <input type="email" placeholder="Digite seu e-mail" class="input" v-model="email">
                <hr>
                <button class="button is-success" @click="updade">Editar</button>
            
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

        axios.get('http://localhost:8086/user/' + this.$route.params.id, req).then(res => {
            
            this.id = res.data.id;
            this.name = res.data.name;
            this.email = res.data.email;

        }).catch( err => {
            console.log(err.response);
            this.$router.push({ name: 'Users' });
        });  
    },
    data() {
        return {
            name: '',
            email: '',
            id: -1,
            error: undefined 
        }
    },
    methods: {
        updade() {

            var req = {
                headers: {
                    Authorization: 'Bearer ' + localStorage.getItem('token')
                }
            }

            axios.put('http://localhost:8086/user', {
                id: this.id,
                name: this.name,
                email: this.email
            }, req).then(res => {
                console.log(res);
                this.$router.push({ name: 'Users' });
            }).catch( err => {
                var msgError = err.response.data.err;
                this.error = msgError;
            });
        }
    }
}
</script>

<style scoped>

</style>