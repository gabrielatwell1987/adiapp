<script>
// @ts-nocheck

    import { onMount } from 'svelte';
    import axios from 'axios';
    import { Spinner } from 'sveltestrap';

    const table = document.querySelector('tr');

    let users = [];
    let searchTerm = '';
    let isLoading = false;

    const getData = async () => {
        try {
            const res = await axios.get(
                `https://api.github.com/search/users?q=${searchTerm}`
            );
            users = res.data.items;
            isLoading = false;
        } catch (e) {

        }
    };

    onMount(getData());

    const handleSubmit = event => {
        event.preventDefault();
        isLoading = true;
        getData();
    };
</script>

<main>
    
    
    <div class="content">
        <img src="github.svg" alt="">

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
                            <td><img alt={user.login} src={user.avatar_url} width="75" height="75"></td>
                            <td><b>{user.login}</b></td>
                            <td><a href="{user.html_url}">{user.html_url}</a></td>
                        </tr>
                    {/each}
                </tbody>
            </table>
        </div>
    </div>
</main>

<style>
    img {
        width: 50%;
        margin-inline: auto;
        object-fit: cover;
        margin-bottom: 15%;
    }
    
    input {
        width: 50%;
        margin-inline: auto;
        padding: 10px;
        border-radius: 15px;
        border: 1px solid #ccc;
        margin-bottom: 10px;
    }

    table {
        position: relative;
    }

    .content {
        scale: 1;
        margin-inline: auto;
    }

    main {
        position: relative;
        margin-inline: auto;
        width: 100%;
        max-width: 400px;
        text-align: center;
    }

    
    @media (max-width: 500px) {
        .table {
            max-width: 100%;
        }
        
        .content {
            overflow-x: auto;
            width: 99%;
            scale: 1;
            margin-inline: auto;
            /* margin-right: 10%; */
        }

        table tr {
            margin-inline: auto;
        }

        td {
            margin: 0;
            padding: 0;
        }

        input {
            width: 100%;
        }
    }
</style>

