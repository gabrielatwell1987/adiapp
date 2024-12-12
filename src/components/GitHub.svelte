<script>
  // @ts-nocheck

  import { onMount } from "svelte";
  import axios from "axios";
  import { Spinner } from "sveltestrap";

  const table = document.querySelector("tr");

  let users = [];
  let searchTerm = "";
  let isLoading = false;

  const getData = async () => {
    try {
      const res = await axios.get(
        `https://api.github.com/search/users?q=${searchTerm}`
      );
      users = res.data.items;
      isLoading = false;
    } catch (e) {}
  };

  onMount(getData());

  const handleSubmit = (event) => {
    event.preventDefault();
    isLoading = true;
    getData();
  };
</script>

<main>
  <div class="content">
    <img src="github.svg" alt="" />

    <form on:submit={handleSubmit}>
      <input type="text" bind:value={searchTerm} placeholder="search" />

      <button type="submit">Submit</button>
    </form>

    {#if isLoading}
      <Spinner color="primary" />
    {/if}

    <div class="table">
      <table>
        <tbody>
          {#each users as user}
            <tr>
              <td>
                <img
                  alt={user.login}
                  src={user.avatar_url}
                  width="75"
                  height="75"
                  class="avatar"
                />
              </td>

              <td><b>{user.login}</b></td>

              <td><a href={user.html_url}>{user.html_url}</a></td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow-x: hidden;
    background-color: #f5f5f5;
  }

  main {
    margin: 0 auto;
    width: 100%;
    max-width: 600px;
    text-align: center;
    background-color: #fff;
    padding: 1rem;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
  }

  .content {
    margin: 0 auto;
    width: 100%;
    max-width: 224px;
    background-color: #fff;
    overflow-x: hidden;
  }

  form {
    margin-bottom: 1rem;
  }

  .table {
    overflow-x: auto;
    margin: 1rem auto;
    max-width: 100%;
    background-color: #fff;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    background-color: #fff;
    margin: 0 auto;
  }

  td {
    padding: 0.5rem;
    text-align: left;
    border-bottom: 1px solid #ddd;
    word-wrap: break-word;
  }

  img {
    display: block;
    margin: 0 auto 2rem;
    max-width: 100%;
    height: auto;
  }

  input {
    width: calc(100% - 2rem);
    max-width: 400px;
    padding: 0.75rem;
    margin: 0 auto 1rem;
    display: block;
    border-radius: 15px;
    border: 3px solid #222;
    background-color: #fff;
    color: #222;
  }

  input::placeholder {
    color: #222;
    font-weight: 600;
    letter-spacing: 3px;
  }

  button {
    background-color: #242424;
    color: #fff;
    font-size: clamp(1.1rem, 1.25vw, 1.25rem);
    font-weight: 600;
    letter-spacing: 3px;
  }

  button:hover {
    background-color: #fff;
    color: #242424;
  }

  @media (max-width: 500px) {
    .content {
      scale: 0.8;
      overflow-x: hidden;
    }

    table {
      font-size: 0.9rem;
    }

    input {
      width: calc(100% - 2rem);
    }
  }
</style>
