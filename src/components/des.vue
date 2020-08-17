<template>
  <div class="hello">
    <form>
      <div>
        <label>
          Mensaje
          <input type="text" v-model="data.message" />
        </label>
        <label v-show="errors.message">
          El mensaje no puede estar vacío
        </label>
      </div>
      <div>
        <label>
          Clave
          <input v-model="data.password" />
        </label>
        <label v-show="errors.password">
          La clave debe tener al menos 6 caracter
        </label>
      </div>

      <button @click.prevent="encrypt" type="submit">Encriptar</button>
      <button @click.prevent="decrypt" type="submit">Desencriptar</button>
    </form>
    <div v-if="type === 'decryption'">
      <label>
        HEX
        <input v-model="result.hex" />
      </label>
    </div>
    <div v-if="type !== ''">
      <label>
        ASCII
        <input v-model="result.ascii" />
      </label>
    </div>
    <div v-if="type === 'encryption'">
      <label>
        Salt
        <input v-model="result.salt" />
      </label>
    </div>
    <div v-if="type === 'encryption'">
      <label>
        IV
        <input v-model="result.iv" />
      </label>
    </div>
  </div>
</template>

<script>
import CryptoJS from "crypto-js"

export default {
  data() {
    return {
      data: {
        message: "",
        password: "",
      },
      type: "",
      errors: {
        message: false,
        password: false,
      },
      result: {
        ascii: "",
        hex: "",
        salt: "",
        iv: "",
      },
    }
  },
  methods: {
    encrypt() {
      if (!this.validation()) return
      this.type = "encryption"
      var encrypted = CryptoJS.DES.encrypt(
        this.data.message,
        this.data.password
      )
      this.result.salt = encrypted.salt.toString()
      this.result.iv = encrypted.iv.toString()
      this.result.ascii = encrypted.toString()
    },
    decrypt() {
      if (!this.validation()) return
      this.type = "decryption"
      this.result.hex = CryptoJS.DES.decrypt(
        this.data.message,
        this.data.password
      )
      this.result.ascii = this.result.hex.toString(CryptoJS.enc.Utf8)
    },
    validation() {
      this.errors.message = this.data.message.length == 0
      this.errors.password = this.data.password.length < 6
      return !this.errors.message && !this.errors.password
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
