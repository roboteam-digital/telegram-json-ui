<!-- https://github.com/sveltejs/svelte-virtual-list -->

<script>
	import VirtualList from '../lib/VirtualList.svelte';
	// import items from '../lib/data.js';
	import ListItem from '../lib/ListItem.svelte';

	import data from '../../../example/telegram-test-json/result.json'
	
	const get_messages = () => data.messages

	let json_messages = get_messages()

	const extract_text = (json_text) => {
		return typeof json_text === 'string' ?
			json_text :
			json_text.map(e => {
				return typeof e === 'string' ? 
					e :
					e.text
			}).join('\n')
	}

	// TODO Load avarars from from Telegram API or HTML export
	const get_avatar = (item) => {
		const default_avatar = 'https://api.lorem.space/image/face?hash=92310'

		return default_avatar
	}

	let messages = json_messages.map(item => ({
		...item,
		avatar: get_avatar(item),
		// plain_text: extract_text(item.text) + '',
		plain_text: extract_text(item.text),
	}))

	let searchTerm = '';
	
	$: filteredList = messages.filter(item => {
		const lower_case_search_term = searchTerm.toLowerCase()
		return item.from    && item.from      .toLowerCase().indexOf(lower_case_search_term) !== -1 ||
			item.actor      && item.actor     .toLowerCase().indexOf(lower_case_search_term) !== -1 ||
			item.plain_text && item.plain_text.toLowerCase().indexOf(lower_case_search_term) !== -1 ||
			item.action     && item.action    .toLowerCase().indexOf(lower_case_search_term) !== -1
	});

	let start;
	let end;
	
</script>

<div class="navbar bg-base-100">
	<!-- <a class="btn btn-ghost normal-case text-xl">daisyUI</a> -->
	<span>Filter:</span>
	<input
	type="text"
	placeholder="Search: Messages and Authors, Actions and Actors"
	class="input input-bordered input-primary w-full max-w-xs"
	bind:value={searchTerm}
>

</div>

<!-- <h1>Virtual list</h1>
<p>Instead of rendering all your data, &lt;VirtualList&gt; just renders the bits that are visible, keeping your page nice and light.</p>
<p>The source code for the component is <a href='https://github.com/sveltejs/svelte-virtual-list'>here</a>.</p> -->


<!-- Filter: <input bind:value={searchTerm} /> -->
<!-- {searchTerm} -->

<div class='container'>
	<VirtualList items={filteredList} bind:start bind:end let:item>
		<ListItem {...item}/>
	</VirtualList>
	<p>showing items {start}-{end}</p>
</div>

<style>
	.container {
		border-top: 1px solid #333;
		border-bottom: 1px solid #333;
		min-height: 200px;
		height: calc(100vh - 15em);
	}
</style>