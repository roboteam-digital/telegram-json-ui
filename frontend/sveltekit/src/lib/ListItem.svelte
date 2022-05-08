<script>
	export let avatar;
	export let from;
	export let actor;
	export let action;
	export let media_type;
	export let plain_text;
	export let date;
	export let mime_type;
	export let file;
	export let thumbnail;
	export let photo
	let dir = '../../static/example/telegram-test-json/';
</script>

<!-- <div class="card bg-base-100 shadow-xl">
	<div class="card-body">
		<div class="avatar">
			<div class="w-16 rounded-full">
			  <img src="{avatar}" alt="{from}" />
			</div>
		</div>
		<h2>{from}</h2>
		<p>{text}</p>
	</div>
</div> -->

<div class='card'>
	<span>{date}</span>
	<div class="avatar">
		<div class="w-16 rounded-full">
		  <img src="{avatar}" alt="{from}" />
		</div>
	</div>
	<!-- <span class='avatar' style='background: url({avatar})' ></span> -->
	<h2>{from || actor}</h2>
	<p>
		{#if action}
			<span>Action: <pre>{action}</pre></span>
		{:else if photo}
			<img src={dir+photo} type="image/jpg" alt="404">
		{:else if mime_type}
			<span>Media: <pre>{media_type}</pre></span>
			{#if RegExp("video*").test(mime_type)}
				<video controls>
					<track kind="captions">
					<source src={dir+file} type={mime_type}>
				</video>
			{:else if RegExp("audio*").test(mime_type)}
				<audio controls>
					<source src={dir+file} type={mime_type}>
				</audio>
			{:else if RegExp("image*").test(mime_type)}
				<img src={dir+file} type={mime_type} alt="404">
			{/if}
		{:else if media_type=="sticker"}
			<!-- <video >
				<track kind="captions">
				<source src={dir+file} type="video/tgs">
			</video>	 -->
			<img src={dir+thumbnail} alt="404">
		{/if}
		{@html plain_text.replace(/(?:\r\n|\r|\n)/g, '<br />')}		
	</p>
</div>

<style>
	.card {
		position: relative;
		margin: 0.5em;
		padding: 0.5em 0.5em 0.5em 6em;
		border: 1px solid #eee;
		border-radius: 4px;
		box-shadow: 2px 2px 4px rgba(0,0,0,0.1);
		min-height: 5em;
	}

	.card::after {
		clear: both;
		display: block;
	}

	.avatar {
		position: absolute;
		width: 5em;
		height: 5em;
		left: 0.5em;
		top: 0.5em;
		border-radius: 50%;
		background: no-repeat 50% 50%;
		background-size: cover;
	}

	h2 {
		margin: 0 0 0.5em 0;
		font-size: 16px;
	}

	p {
		margin: 0;
		font-size: 14px;
	}
</style>