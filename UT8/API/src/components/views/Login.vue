<script setup>
import AuthUsuario from "../AuthUsuario.vue";
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const emit = defineEmits(["usuario-verificado"]);
let errorMsg = ref(null);
const tryAuthUser = async (userData) => {
  if (userData.email && userData.password) {
    try {
      const response = await fetch("api/v1/usuarios");
      const data = await response.json();

      const usuarioEncontrado = data.usuarios.find(
        (usuario) =>
          usuario.email === userData.email &&
          usuario.password === userData.password
      );

      if (usuarioEncontrado) {
        const userData = {
          id: usuarioEncontrado.id,
          email: usuarioEncontrado.email,
        };
        emit("usuario-verificado", userData);
        errorMsg.value = "";
        router.push("/");
      } else {
        errorMsg.value = "Email o contraseña incorrectos.";
      }
    } catch (error) {
      errorMsg.value = "Error";
    }
  }
};
</script>
<template>
  <router-link to="/">Inicio</router-link>
  <AuthUsuario
    titulo="Iniciar sesion"
    @submit="tryAuthUser"
    :error="errorMsg"
  />
</template>
