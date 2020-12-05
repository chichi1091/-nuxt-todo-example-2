<template>
    <v-layout
    justify-center
    align-center
    >
        <div class="login-form">
            <form @submit.prevent="login">
                <img src="logo.jpg">
                <p class="error" v-if="error">{{ error }}</p>
                <p>
                    <input type="text" v-model="email" placeholder="email" name="email" class="textlines"/>
                </p>
                <p>
                    <input type="text" v-model="password" placeholder="password" name="password" class="textlines"/>
                </p>
                <p>
                    <v-btn
                    light
                    block
                    elevation="2"
                    @click="submit"
                    >ログイン</v-btn>
                </p>
            </form>
        </div>
    </v-layout>
</template>

<script>
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
        try {
          await this.$store.dispatch("login", {
            email: this.email,
            password: this.password
          })
          this.$router.push("/")
        } catch(e) {
          this.error = e.message
        }
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