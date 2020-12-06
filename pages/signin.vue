<template>
    <v-layout
    justify-center
    align-center
    >
        <div class="login-form">
            <form @submit.prevent="login">
                <p class="error" v-if="error">{{ error }}</p>
                <img src="logo.jpg">
                <p>
                    <input type="text"
                    v-model="email"
                    placeholder="email"
                    name="email"
                    class="textlines"/>
                </p>
                <p>
                    <input type="password"
                    v-model="password"
                    placeholder="password"
                    name="password"
                    class="textlines"/>
                </p>
                <p>
                    <v-btn
                    light
                    block
                    elevation="2"
                    type="submit"
                    >ログイン</v-btn>
                </p>
                <p>
                    <v-btn
                        block
                        small
                        @click="signup"
                    >新規作成</v-btn>
                </p>
            </form>
        </div>
    </v-layout>
</template>

<script>
import axios from 'axios'

export default {
    layout(context) {
        return 'login'
    },
    data() {
      return {
        error: null,
        email: "",
        password: "",
      }
    },
    methods: {
      async login() {
        const request = {
            email: this.email,
            password: this.password
        }
        await axios.post('/api/accounts/auth', request)
        .then(response => {
            console.log(response);
            if(response.status === 200) {
                this.$cookies.set('token', response.data.bearer_token);
                this.$router.push("/todos");
                return;
            }
            this.error = 'サインインに失敗しました';
        }).catch(error => {
            console.log(error);
            this.error = 'サインインに失敗しました';
        });
      },
      signup() {
        this.$router.push("signup")
      }
    }
  }
</script>

<style scoped>
.textlines {
  border: 2px solid white;
  border-radius: 0.67em;
  padding: 0.5em;
  background-color: snow;
  font-size: 1em;
  line-height: 1.2;
}
</style>