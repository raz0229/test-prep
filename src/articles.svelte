<script>
	import Article from "./Article.svelte";

	export let articles;
	let dmInp;
	export let statusBuffer = new Set();
	export let contentLength;
	export let temp_articles;

	const filterDomain = (e, inp) => {
		if (e.keyCode == 8) articles = temp_articles;
		articles = articles.filter((x) => {
			return x.domain.trim().includes(inp.trim());
		});
	};
</script>

{#if articles}
	<div class="options">
		<!-- Switch -->
		<div
			id="sw"
			class="switch"
			style="text-align: start; padding-left: 15px;"
		>
			<label>
				Latest
				<input
					type="checkbox"
					checked
					on:change={() => {
						articles = articles.reverse();
						statusBuffer.clear();
					}}
				/>
				<span class="lever" />
				Oldest
			</label>
		</div>

		<!-- Filter -->
		<div id="if" class="input-field col s6">
			<input
				id="last_name"
				bind:value={dmInp}
				on:keyup={(e) => filterDomain(e, dmInp)}
				type="text"
				class="validate"
			/>
			<label for="last_name">Filter Domain</label>
		</div>
	</div>
	{#each articles as article, i}
		{#if i < contentLength}
			<ul>
				<li>
					<Article {article} {contentLength} {statusBuffer} />
				</li>
			</ul>
		{/if}
	{/each}
{:else}
	<div class="preloader-wrapper big active center">
		<div class="spinner-layer spinner-blue-only">
			<div class="circle-clipper left">
				<div class="circle" />
			</div>
			<div class="gap-patch">
				<div class="circle" />
			</div>
			<div class="circle-clipper right">
				<div class="circle" />
			</div>
		</div>
	</div>
{/if}

<style>
	@keyframes fade-in {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}

	li {
		list-style-type: georgian;
	}

	.options {
		display: flex;
		align-items: center;
	}
	.options #sw {
		flex: 1 1 0px;
		margin: 0;
	}
	.options #if {
		flex: 2 2 0px;
		margin: 0;
		padding-right: 10px;
	}
</style>
