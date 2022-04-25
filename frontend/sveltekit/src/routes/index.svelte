<!-- https://github.com/sveltejs/svelte-virtual-list -->

<script>
	import VirtualList from '../lib/VirtualList.svelte';
	// import items from '../lib/data.js';
	import ListItem from '../lib/ListItem.svelte';

	import data from '../../static/example/telegram-test-json/result.json'
	
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
	let lowerBoundDate = '';
	let upperBoundDate = '';

	function termFilter(searchTerm, itemField){
		return itemField    && itemField     .toLowerCase().indexOf(searchTerm) !== -1
	}

	function dateFilter(lb, ub, d){
		const DAY = 86400
		return (Number.isNaN(lb) || d>=lb) && (Number.isNaN(ub) || d<=ub + DAY)
	}
	
	$: filteredList = messages.filter(item => {
		const lower_case_search_term = searchTerm.toLowerCase()
		let termFilterWithTerm = termFilter.bind(null, lower_case_search_term)
		let dateFilterWithBounds = dateFilter.bind(null, Date.parse(lowerBoundDate), Date.parse(upperBoundDate))
		return (termFilterWithTerm(item.from) ||
				 termFilterWithTerm(item.actor) ||
				 termFilterWithTerm(item.plain_text) ||
				 termFilterWithTerm(item.action)) &&
				 dateFilterWithBounds(Date.parse(item.date))
	});

	let start;
	let end;

	const input_json_url = 'https://telegram-json-ui.netlify.app/example/telegram-test-json/result.json'
	const input_html_url = 'https://telegram-json-ui.netlify.app/example/telegram-test-html/messages.html'
	
</script>

<div class="alert shadow-lg">
	<ul>
		<li>See demo input JSON there: <a target="_blank" class="link link-primary" href="{input_json_url}">result.json</a></li>
		<li>And see as it originally rendered in HTML: <a target="_blank" class="link link-primary" href="{input_html_url}">messages.html</a></li>
	</ul>
</div>

<div class="navbar bg-base-100">
	<!-- <a class="btn btn-ghost normal-case text-xl">daisyUI</a> -->
	<span>Chat:</span>
	<span>Filter:</span>
	<input
	type="text"
	placeholder="Search: Messages and Authors, Actions and Actors"
	class="input input-bordered input-primary w-full max-w-xs"
	bind:value={searchTerm}
	>
	<input
	type="date"
	class="input input-bordered input-primary w-full max-w-xs"
	bind:value={lowerBoundDate}
	>
	<input
	type="date"
	class="input input-bordered input-primary w-full max-w-xs"
	bind:value={upperBoundDate}
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