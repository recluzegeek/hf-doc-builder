<script lang="ts">
	import type { SvelteComponent } from "svelte";
	import DropdownMenu from "./DropdownMenu.svelte";
	import IconCaretDown from "./IconCaretDown.svelte";

	export let classNames = "";
	export let btnClassNames = "";
	export let btnIcon: typeof SvelteComponent | undefined = undefined;
	export let btnIconClassNames = "";
	export let btnLabel = "";
	export let forceMenuAlignement: "left" | "right" | undefined = undefined;
	export let menuClassNames = "";
	export let noBtnClass: boolean | undefined = undefined;
	export let selectedValue: string | undefined = undefined;
	export let useDeprecatedJS = true;
	export let withBtnCaret = false;

	let element: HTMLElement | undefined = undefined;
	let isOpen = false;

	function onClose(e: Event) {
		if (e.target) {
			if ((e.target as HTMLElement | undefined)?.className.includes("do-not-close-dropdown")) {
				return;
			}
		}
		isOpen = false;
	}
</script>

<div class="relative {classNames} {useDeprecatedJS ? 'v2-dropdown' : ''}" bind:this={element}>
	<!-- Button -->
	<button
		class="
			{btnClassNames}
			{!noBtnClass ? 'cursor-pointer w-full btn text-sm' : ''}
			{useDeprecatedJS ? 'v2-dropdown-button' : ''}"
		on:click={() => (isOpen = !isOpen)}
		type="button"
	>
		<!-- The "button" slot can overwrite the defaut button content -->
		{#if $$slots.button}
			<slot name="button" />
		{:else}
			{#if btnIcon}
				<svelte:component this={btnIcon} classNames="mr-1.5 {btnIconClassNames}" />
			{/if}
			{btnLabel}
			{#if withBtnCaret}
				<IconCaretDown classNames="-mr-1 text-gray-500" />
			{/if}
		{/if}
	</button>
	<!-- /Button -->
	<!-- Menu -->
	{#if isOpen || useDeprecatedJS}
		<DropdownMenu
			classNames="{menuClassNames} {useDeprecatedJS ? 'v2-dropdown-menu hidden' : ''}"
			dropdownElement={element}
			forceAlignement={forceMenuAlignement}
			{onClose}
		>
			<slot name="menu" />
		</DropdownMenu>
	{/if}
	<!-- Menu -->
</div>
