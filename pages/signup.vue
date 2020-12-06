<template>
    <v-layout
    justify-center
    align-center
    >
        <v-card>
            <v-card-title>
            アカウント新規作成
            </v-card-title>
            <div class="signup-form">
                <form @submit.prevent="signup">
                    <p v-if="errors.length">
                        <ul>
                            <li style="color: red;"
                            v-for="(error, key) in errors"
                            :key="key">{{ error }}
                            </li>
                        </ul>
                    </p>

                    <p v-if="infos.length">
                        <ul>
                            <li style="color: green;"
                            v-for="(info, key) in infos"
                            :key="key">{{ info }}
                            </li>
                        </ul>
                    </p>

                    <p>
                        <input type="text"
                        v-model="name"
                        placeholder="name"
                        name="name"
                        class="textlines"/>
                    </p>
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
                        >新規作成</v-btn>
                    </p>
                </form>
            </div>
        </v-card>
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
        errors: [],
        infos: [],
        name: "",
        email: "",
        password: "",
      }
    },
    methods: {
      async signup() {
        this.errors = [];
        if (!this.name) this.errors.push('Nameが未入力です');
        if (!this.email) this.errors.push('EMailが未入力です');
        else {
            const regexp = /^[A-Za-z0-9]{1}[A-Za-z0-9_.-]*@{1}[A-Za-z0-9_.-]{1,}\.[A-Za-z0-9]{1,}$/;
            if(!regexp.test(this.email)) this.errors.push('EMail形式に誤りがあります');
        }
        if (!this.password) this.errors.push('Passwordが未入力です');
        if(this.errors.length > 0) return;

        const request = {
            name: this.name,
            email: this.email,
            password: this.password
        }
        await axios.post('/api/accounts', request)
        .then(response => {
            this.infos.push('新規作成しました。サインイン画面へ遷移します。');
            setTimeout(() => {this.$router.push("/signin")}, 2000);
        })
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