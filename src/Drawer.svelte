<script lang='ts'>
  import { createEventDispatcher, onDestroy } from 'svelte';
  export let open;

	const dispatch = createEventDispatcher();
	const close = () => dispatch('close');

	let aside;

	const handle_keydown = e => {
		if (e.key === 'Escape') {
			close();
			return;
		}

		if (e.key === 'Tab') {
			// trap focus
			const nodes = aside.querySelectorAll('*');
			const tabbable = Array.from(nodes).filter((n: any) => n.tabIndex >= 0);

			let index = tabbable.indexOf(document.activeElement);
			if (index === -1 && e.shiftKey) index = 0;

			index += tabbable.length + (e.shiftKey ? -1 : 1);
			index %= tabbable.length;

			(tabbable[index] as any).focus();
			e.preventDefault();
		}
	};

	const previously_focused = typeof document !== 'undefined' && document.activeElement;

	if (previously_focused) {
		onDestroy(() => {
			(previously_focused as any).focus();
		});
	}
</script>

<div class='aside-background' on:click={close}></div>

<aside class={`aside bg-transparent ${open ? 'open' : ''}`}>
  <slot></slot>
</aside>

<style>
	.aside-background {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
    background: rgba(0, 0, 0, 0.253);
    z-index: 12;
	}

	.aside {
    position: absolute;
    right: -100%;
    width: 30%;
    z-index: 12;
    transition: right 1s cubic-bezier(.46,.03,.52,.96);
  }
  
  .open {
    right: 0;
  }
</style>
