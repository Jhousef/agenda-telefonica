<template>
    <div class="container-fluid">
        <div class="row justify-content-center align-items-center vh-100">
            <div class="col-md-4">
                <div class="card">
                    <div class="card-body">
                        <h1 class="text-center mb-4">{{ isLogin ? 'Login' : 'Cadastro' }}</h1>
                        <form @submit.prevent="isLogin ? login() : register()">
                            <div v-if="!isLogin" class="mb-3">
                                <label for="name" class="form-label">Nome</label>
                                <input type="text" class="form-control" id="name"
                                    v-model="name" required>
                            </div>
                            <div class="mb-3">
                                <label for="email" class="form-label">Email</label>
                                <input type="email" class="form-control" id="email" v-model="email" required>
                            </div>
                            <div class="mb-3">
                                <label for="password" class="form-label">Password</label>
                                <input type="password" class="form-control" id="password" v-model="password" required>
                            </div>
                            
                            <button type="submit" class="btn btn-primary w-100">
                                {{ isLogin ? 'Login' : 'Cadastrar' }}
                            </button>
                        </form>
                        <button class="btn btn-link mt-2" @click="toggleLoginMode">{{ isLogin ? 'Cadastre-se' : 'Login'
                            }}</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import TodoService from '@/services/todo-services';
import router from '@/router';

export default {
    name: 'Auth',
    data() {
        return {
            email: '',
            password: '',
            name: '',
            isLogin: true
        };
    },
    methods: {
        async login() {
            try {
                const config = {
                    headers: {
                        "Access-Control-Allow-Origin": "*",
                        "Access-Control-Allow-Headers": "Origin, X-Requested-With, Content-Type, Accept",
                        "Access-Control-Allow-Methods": "PUT, POST, GET, DELETE, OPTIONS",
                        "Content-Type": "application/json",
                    }
                };

                const response = await TodoService.login({
                    email: this.email,
                    password: this.password
                }, config);

                console.log('Token:', response.data.token);

                localStorage.setItem('authToken', response.data.token);

                router.push({ name: 'todo.index' });
            } catch (error) {
                console.error('Erro no login:', error);
            }
        },

        async register() {
            try {
                const config = {
                    headers: {
                        "Access-Control-Allow-Origin": "*",
                        "Access-Control-Allow-Headers": "Origin, X-Requested-With, Content-Type, Accept",
                        "Access-Control-Allow-Methods": "PUT, POST, GET, DELETE, OPTIONS",
                        "Content-Type": "application/json",
                    }
                };
                const response = await TodoService.register({
                    email: this.email,
                    password: this.password,
                    name: this.name
                }, config);
                console.log('Token:', response.data);

                localStorage.setItem('authToken', response.data.token);

                router.push({ name: 'todo.index' });
            } catch (error) {
                console.error('Erro no cadastro:', error);

            }
        },
        toggleLoginMode() {
            this.isLogin = !this.isLogin;
        }
    }
};
</script>

<style scoped>
.vh-100 {
    height: 100vh;
}

.card {
    border: none;
    border-radius: 10px;
    box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
}

.card-body {
    padding: 2rem;
}

.btn-primary {
    background-color: #007bff;
    border-color: #007bff;
}

.btn-primary:hover {
    background-color: #0069d9;
    border-color: #0062cc;
}

.btn-primary:focus,
.btn-primary.focus {
    box-shadow: 0 0 0 0.2rem rgba(38, 143, 255, 0.5);
}
</style>