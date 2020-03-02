<script>
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

{#await Promise}
    <p>Fetching Repos ....</p>
{:then repos}
    {#each repos as repo, i}
        <h1> {repo.name} </h1>
    {/each}
{:catch error}
    <h1 style="color: red;">{ error.message }</h1>
{/await}
