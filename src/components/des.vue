<template>
  <div class="flex flex-wrap justify-center m-4">
    <form
      class="w-full max-w-4xl rounded overflow-hidden shadow-lg p-6 bg-white text-center m-4"
    >
      <h1 class="font-semibold mb-6 text-2xl lg:text-3xl xl:text-4xl">
        Encriptador DES
      </h1>
      <div class="md:flex md:items-center">
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            Mensaje
          </label>
        </div>
        <div class="md:w-5/6">
          <input
            class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500"
            id="inline-full-name"
            type="text"
            v-model="data.message"
          />
        </div>
      </div>

      <div class="md:flex mb-6">
        <div class="md:w-1/6"></div>
        <div class="md:w-5/6">
          <p class="text-red-500 text-sm italic" v-show="errors.message">
            El mensaje debe tener al menos 1 caracter
          </p>
        </div>
      </div>
      <div class="md:flex md:items-center">
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            Clave
          </label>
        </div>
        <div class="md:w-5/6">
          <input
            class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500"
            id="inline-full-name"
            type="text"
            v-model="data.password"
          />
        </div>
      </div>
      <div class="md:flex mb-6">
        <div class="md:w-1/6"></div>
        <div class="md:w-5/6">
          <p class="text-red-500 text-sm italic" v-show="errors.password">
            La clave debe tener al menos 6 caracteres.
          </p>
        </div>
      </div>

      <div class="md:flex justify-center">
        <button
          class="my-1 w-full md:w-1/5 mx-0 md:mx-3 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          @click.prevent="encrypt"
          type="submit"
        >
          Encriptar
        </button>
        <button
          class="my-1 w-full md:w-1/5 mx-0 md:mx-3 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          @click.prevent="decrypt"
          type="submit"
        >
          Desencriptar
        </button>
        <button
          class="my-1 w-full md:w-1/5 mx-0 md:mx-3 border-2 border-gray-400 text-gray-500 bg-white hover:bg-gray-400 hover:text-white focus:outline-none font-bold py-2 px-4 rounded"
          @click.prevent="clean"
          type="submit"
        >
          Limpiar
        </button>
      </div>
    </form>

    <div
      class="w-full max-w-4xl bg-red-200 rounded overflow-hidden shadow-lg p-6 bg-white text-center m-4"
      v-if="errors.decryption"
    >
      <h1 class="text-2xl text-red-400 font-semibold mb-3">
        El texto no se puede desencriptar
      </h1>
      <p class="text-gray-700">El mensaje o la clave son incorrectas</p>
    </div>

    <div
      class="w-full max-w-4xl rounded overflow-hidden shadow-lg p-6 bg-white text-center m-4"
      v-if="type !== '' && !errors.decryption"
    >
      <h1 class="text-2xl font-semibold mb-3" v-if="type === 'encryption'">
        Texto encriptado
      </h1>
      <h1 class="text-2xl font-semibold mb-3" v-if="type === 'decryption'">
        Texto desencriptado
      </h1>
      <div class="md:flex md:items-center mt-6" v-if="type === 'decryption'">
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            HEX
          </label>
        </div>
        <div class="md:w-5/6">
          <input
            class="bg-gray-100 border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight  focus:outline-none "
            v-model="result.hex"
            readonly
          />
        </div>
      </div>

      <div class="md:flex md:items-center mt-6">
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            ASCII
          </label>
        </div>
        <div class="md:w-5/6">
          <input
            class="bg-gray-100 border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight  focus:outline-none "
            v-model="result.ascii"
            readonly
          />
        </div>
      </div>

      <div class="md:flex md:items-center mt-6" v-if="type === 'encryption'">
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            Salt
          </label>
        </div>
        <div class="md:w-2/6">
          <input
            class="bg-gray-100 border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight  focus:outline-none "
            v-model="result.salt"
            readonly
          />
        </div>
        <div class="md:w-1/6">
          <label
            class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
            for="inline-full-name"
          >
            IV
          </label>
        </div>
        <div class="md:w-2/6">
          <input
            class="bg-gray-100 border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight  focus:outline-none "
            v-model="result.iv"
            readonly
          />
        </div>
      </div>
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
        decryption: false,
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
      if (this.result.ascii.toString() === "") {
        this.errors.decryption = true
      }
    },
    clean() {
      this.data = {
        message: "",
        password: "",
      }
      this.type = ""
      this.errors = {
        message: false,
        password: false,
        decryption: false,
      }
    },
    validation() {
      this.errors.decryption = false
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
