<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import { Spinner } from "sveltestrap";

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

      const grid = document.querySelector(".grid");
      if (grid) {
        grid.scrollTop = 0;
      }
    } catch (e) {}
  };

  onMount(() => getData());

  const handleSubmit = (event) => {
    event.preventDefault();
    isLoading = true;
    getData();
  };
</script>

<main>
  <div class="content">
    <a href="https://github.com/gabrielatwell1987/github">
      <img src="github.svg" alt="" />
    </a>

    <form on:submit={handleSubmit}>
      <input type="text" bind:value={searchTerm} placeholder="search" />

      <button type="submit">Submit</button>
    </form>

    {#if isLoading}
      <Spinner color="primary" />
    {/if}

    <div class="grid">
      {#each users as user}
        <div class="grid-item">
          <img
            alt={user.login}
            src={user.avatar_url}
            width="75"
            height="75"
            class="avatar"
          />

          <b>{user.login}</b>

          <a href={user.html_url} target="_blank">{user.html_url}</a>
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
  :global(html, body) {
    margin: 0;
    padding: 0;
    width: 100vw;
    height: 100vh;
    overflow-x: hidden;
    background-color: #f5f5f5;
  }

  main {
    margin-inline: auto;
    width: 100%;
    height: 100%;
    max-width: 1200px;
    text-align: center;
    background-color: #f5f5f5;
    padding: 2rem 1rem;
    border-radius: 8px;
    padding: 2rem 1rem;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    min-height: 100vh;
    overflow-y: auto;
  }

  .content {
    margin: 0 auto;
    width: 35vw;
    max-width: 1200px;
    background-color: #f5f5f5;
    overflow-x: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    & .avatar {
      padding-top: 0;
    }
  }

  img {
    margin-bottom: 2rem;
    width: 50%;
    max-height: fit-content;
    padding-top: 15rem;
  }

  form {
    margin-bottom: 1rem;
    width: 100%;
    max-width: 1200px;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    place-items: center;
    gap: 1rem;
    margin: 1rem auto;
    justify-content: center;
    max-height: calc(100vh - 200px);
    overflow-y: auto;
    padding: 2rem;
    background: #f5f5f5;
    border-radius: 8px;
  }

  .grid-item {
    background: #fff;
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    text-align: center;
    width: fit-content;

    & img {
      display: block;
      margin: 0 auto 1rem;
      border-radius: 50%;
    }

    & a {
      display: block;
      color: #007bff;
      text-decoration: none;
      margin-top: 0.5rem;

      &:hover {
        border-bottom: 1px solid #242424;
      }
    }
  }

  input {
    width: 100%;
    max-width: 1200px;
    padding: 0.5rem;
    margin-inline: auto;
    display: block;
    border-radius: 15px;
    border: 2px solid #222;
    background-color: #fff;
    color: #222;
    text-align: center;

    &:focus {
      outline: none;
    }

    &::placeholder {
      color: #222;
      font-weight: 600;
      letter-spacing: 3px;
    }
  }

  button {
    background-color: #242424;
    color: #fff;
    font-size: clamp(1.1rem, 1.25vw, 1.25rem);
    font-weight: 600;
    letter-spacing: 3px;
    padding: 0.5rem 1rem;

    &:hover {
      background-color: #fff;
      color: #242424;
    }
  }

  a {
    color: #007bff;
    text-decoration: none;

    &:hover {
      color: #242424;
    }
  }

  @media (max-width: 500px) {
    .content {
      overflow-x: hidden;
      margin-inline: auto;
      width: 70vw;
      height: 100vh;
      margin-top: 1rem;
    }

    img {
      max-width: 50%;
      padding-top: 0;
    }

    .grid {
      gap: 0.5rem;
      min-height: fit-content;
    }

    .grid-item {
      padding: 0.75rem;
    }

    input {
      width: 100%;
      max-width: none;
    }

    .avatar {
      width: 5em;
      height: 5em;
    }
  }
</style>
