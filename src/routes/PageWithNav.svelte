<script lang="ts">
	import { onMount } from 'svelte';

	import HeightUpdater from './HeightUpdater.svelte';
	import ScrollUpdater from './ScrollUpdater.svelte';

	import { TouchMode, customTouchEnd, customTouchMove, customTouchStart } from './touch';
	
	
	import { height } from './height.store';

	export let parent: string | null = null;
	export let overflowHidden: boolean = false; // true for pages with svelte-confetti
	export let hideNav: boolean = false;
	export let touchMode: TouchMode = TouchMode.CLICK_ONLY;

	onMount(() => {
	});
</script>

<ScrollUpdater />
<HeightUpdater />

<div
	class="page-wrapper"
	style="height: {$height}px"
	on:touchstart={customTouchStart(touchMode)}
	on:touchend={customTouchEnd}
	on:touchmove={customTouchMove}
>
	<div
		id="scrollable"
		class="page-content"
		style="height: {$height}px; {overflowHidden ? 'overflow: hidden;' : ''};"
	>
		<slot />
	</div>
</div>

<style lang="scss">
	.page-wrapper {
		// background-image: url('/img/background-pattern.svg?v=2');
		background-size: 300px;
		background-repeat: repeat;

		position: relative;

		padding-top: env(safe-area-inset-top);
		padding-right: env(safe-area-inset-right);
		padding-bottom: env(safe-area-inset-bottom);
		padding-left: env(safe-area-inset-left);
	}

	.page-content {
		z-index: 0;
		overflow-y: auto;
		position: relative;
	}
	.page-nav {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 1;
	}
</style>
