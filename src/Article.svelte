<script>
	export let article;
	export let statusBuffer;
	let status;

	const dateSeparator = (time) => {
		let passed = new Date(time * 1000).toString().substring(4, 15);
		let today = new Date().toString().substring(4, 15);
		let current = new Date().getTime().toString().substring(0, 10);
		let yesterday = new Date((current - 86400) * 1000)
			.toString()
			.substring(4, 15);
		if (passed == today) status = "Today";
		else if (passed == yesterday) status = "Yesterday";
		else status = passed;
		return status;
	};

	const updateStatus = (time) => {
		status = dateSeparator(time);
		statusBuffer.add(status);
		return status;
	};
</script>

{#if !statusBuffer.has(dateSeparator(article.timestamp))}
	<h6 class="center separator">{updateStatus(article.timestamp)}</h6>
{/if}

<div class="keycol">
	<div class="domain">
		<span style="color: #3f51b5;" class="material-icons">
			language
		</span>ㅤ&zwnj;&zwnj; {article.domain}
	</div>
	<div class="text">{article.text}</div>
	<div class="timestamp">
		{new Date(article.timestamp * 1000).toString().substring(4, 24)}
	</div>
	<div class="borderline" />
</div>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Montserrat+Alternates&family=Mukta:wght@500&family=Quicksand&display=swap");

	.borderline {
		padding-bottom: 1rem;
		border-bottom: solid 1px #e1e1e1;
		margin: 0 20px 0 20px;
	}
	.domain {
		display: flex;
		font-weight: bold;
		color: #3f51b5;
		font-size: 18px;
		padding: 10px 20px 5px 20px;
		align-items: center;
		justify-content: start;
	}
	.text {
		padding: 0 15px;
		font-size: 20px;
		color: #939393;
	}
	.timestamp {
		text-align: end;
		padding-right: 15px;
		padding-top: 15px;
	}
	.separator {
		color: rgb(0 0 0 / 30%);
		font-family: "Mukta", sans-serif;
		border-top: solid 12px#f7f7f7;
		padding-top: 1rem;
	}
</style>
