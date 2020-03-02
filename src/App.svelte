<script>
    import Repo from './components/Repo.svelte';

    let greeting = 'Hello World';
    let repos = [];
    let Promise = fetchGithubData();

    async function fetchGithubData() {
        const res = await fetch('https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&per_page=100&page=');
        const data = await res.text();

        if (res.ok) {
            let payload = JSON.parse(data);
            return repos = payload.items;
        } else {
            return new Error(data);
        }
    }
</script>

<div class="container">

    {#await Promise}
        <p>Fetching Repos ....</p>
    {:then repos}
    <table class="table">
        <thead class="thead-dark">
        <tr>
            <th scope="col">Rank</th>
            <th scope="col">Name</th>
            <th scope="col">Stars</th>
            <th scope="col">Avatar</th>
            <th scope="col">Github</th>
        </tr>
        </thead>
    <tbody>
    {#each repos as repo, i}
        <tr class="mt-2">
            <th scope="row">
                {i + 1}
            </th>
            <td>
                {repo.name}
            </td>
            <td>
                {repo.stargazers_count.toLocaleString() }
            </td>
            <td>
                <img class="avatar" src="{repo.owner.avatar_url}" alt="Repo Avatar" />
            </td>
            <td>
                <a href="{repo.html_url}" target="_blank">Link</a>
            </td>
        </tr>
    {/each}
    </tbody>

    </table>

    {:catch error}
        <h1 style="color: red;">{ error.message }</h1>
    {/await}
</div>

<style>
    .avatar {
        max-width: 150px;
        max-height: 75px;
    }
</style>
