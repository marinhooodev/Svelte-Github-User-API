<script lang="ts">
  import Formulario from "./components/Formulario.svelte";
  import Titulo from "./components/Titulo.svelte";
  import Usuario from "./components/Usuario.svelte";
  import type IUsuario from "./interfaces/IUsuario";

  let usuario: IUsuario | null;

  function definirUsuario(event: CustomEvent<IUsuario | null>) {
    usuario = event.detail
    console.log(usuario)
    // event detail é onde o valor do dispach fica armazenado
  }
</script>

<div class="app">
  <header>
    <Titulo />

    <div class="busca-usuario">
      <Formulario on:aoAlterarUsuario={definirUsuario} />
    </div>
  </header>
  {#if usuario}
    <Usuario {usuario} />
    <!-- Props -->
  {/if}
</div>

<style>
  .app {
    max-height: 100vh;
  }

  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .busca-usuario {
    position: relative;
    width: 70%;
  }
</style>
