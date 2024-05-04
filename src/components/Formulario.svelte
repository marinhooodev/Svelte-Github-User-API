<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import type IUsuario from "../interfaces/IUsuario";
  import { buscaRepositorios, buscaUsuario } from "../requests";
  import montaUsuario from "../utils/montaUsuario";
  import Botao from "./Botao.svelte";

  let username: string;
  let statusErro: null | number = null;
  const dispatch = createEventDispatcher<{
    aoAlterarUsuario: IUsuario | null;
  }>();
  // Dispatch mesma coisa do EMIT do Vue

  async function aoSubmeter() {
    const request = await buscaUsuario(username);
    const reposRequest = await buscaRepositorios(username);

    if (!request.ok) {
      statusErro = 404;
      dispatch("aoAlterarUsuario", null);
      // manda um objeto nulo pra div de usuario
      return;
    }

    statusErro = null;
    const response = await request.json();
    const reposResponse = await reposRequest.json();

    dispatch("aoAlterarUsuario", montaUsuario(response, reposResponse));
  }
</script>

<form on:submit|preventDefault={aoSubmeter}>
  <!-- Prevent default faz com que o form não recarregue a página ao ser enviado -->
  <input
    type="text"
    class="input"
    class:erro-input={statusErro === 404}
    bind:value={username}
    placeholder="Pesquise aqui o usuário"
  />

  {#if statusErro === 404}
    <span class="erro">Usuário não encontrado</span>
  {/if}

  <div class="botao-container">
    <Botao> 
      Buscar
    <img src="./lupa.svg" alt="Lupa">
    </Botao>
  </div>
</form>

<style>
  .input {
    padding: 15px 25px;
    width: calc(100% - 8.75rem);
    font-size: 1rem;
    border-radius: 8px;
    border: 1px solid #2e80fa;
    box-shadow: 0px 17px 52px rgba(222, 231, 247, 0.4);
    outline: 0;
  }

  .input::placeholder {
    font-family: "Roboto";
    font-style: italic;
    font-weight: 300;
    font-size: 19.5px;
    line-height: 26px;
    color: #6e8cba;
  }

  .botao-container {
    position: absolute;
    width: 9.625rem;
    right: 0;
    top: 0;
    bottom: 0;
    display: flex;
  }

  .erro {
    position: absolute;
    bottom: -25px;
    left: 0;
    font-style: italic;
    font-weight: normal;
    font-size: 16px;
    line-height: 19px;
    z-index: -1;
    color: #ff003e;
  }

  .erro-input {
    border: 1px solid;
    color: #ff003e;
  }
</style>
