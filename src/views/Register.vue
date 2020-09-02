<template>
    <div>
        <h1>Registro de usuário</h1>
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
                <p>Senha</p>
                <input type="password" placeholder="********" class="input" v-model="password">
                <hr>
                <button class="button is-success" @click="register">Cadastrar</button>
            
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            name: '',
            password: '',
            email: '',
            error: undefined 
        }
    },
    methods: {
        register() {
            axios.post('http://localhost:8086/user', {
                name: this.name,
                email: this.email,
                password: this.password,
            }).then(res => {
                console.log(res);
                this.$router.push({ name: 'Home' });
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