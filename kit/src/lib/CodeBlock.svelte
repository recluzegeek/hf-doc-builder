<script>
	import CopyButton from "./CopyButton.svelte";
	let hideCopyButton = true;
	export let code = "";
	export let highlighted = "";
	export let wrap = false;
	export let classNames = "";

	function handleMouseOver() {
		hideCopyButton = false;
	}
	function handleMouseOut() {
		hideCopyButton = true;
	}
</script>

<div
	class="code-block relative {classNames}"
	on:mouseover={handleMouseOver}
	on:focus={handleMouseOver}
	on:mouseout={handleMouseOut}
	on:blur={handleMouseOut}
>
	<div class="absolute top-2.5 right-4">
		<CopyButton
			classNames="transition duration-200 ease-in-out {hideCopyButton && 'opacity-0'}"
			label="code excerpt"
			value={code}
		/>
	</div>
	<pre class={wrap ? "whitespace-pre-wrap" : ""}>{@html highlighted}</pre>
</div>
