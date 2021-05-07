<script>
	let user = "covid19";

	let constructURL = (user) =>
		"https://newsapi.org/v2/everything?q=" +
		user +
		"&from=2021-04-07&sortBy=publishedAt&apiKey=26839a1bf36e4331bf36eeb47af9ffd0";

	async function fetchNews() {
		let url = constructURL(user);

		var res = await fetch(url);
		res = await res.json();
		let articles = await res.articles;

		return articles;
	}

let promise = fetchNews();

	function handleClick() {
		promise = fetchNews();
	}
</script>

<link
	rel="stylesheet"
	href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css"
	integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh"
	crossorigin="anonymous"
/>

<table class="table">
	<tr>
		<td><input bind:value={user} class="form-control" /> </td>
		<td
			><button on:click={handleClick} class="btn btn-primary">
				fetch news
			</button>
		</td>
	</tr>
</table>

{#await promise}
	<p>...waiting</p>
{:then articles}
	<table class="table">
		{#each articles as article}
			<tr>
				<td>
					<img src={article.urlToImage} width="64" />
				</td>

				<td>
					<a href={article.url}>{article.title}</a>
				</td>
			</tr>
		{/each}
	</table>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
