<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	import { cn } from '$lib/utils';
	import { AlignJustify, XIcon } from 'lucide-svelte';
	import { fly } from 'svelte/transition';
	import { LightSwitch } from '$lib/components/custom';
	import { ARE_Logo } from '$lib/components/custom';
	import { toggleMode } from 'mode-watcher';

	const menuItem = [
		{
			id: 1,
			label: 'Switch Theme',
			href: '#',
			onclick: () => {
				toggleMode();
			}
		},
		{
			id: 2,
			label: 'Live Scores',
			href: '/livescores/countdown',
			onclick: () => {
				CloseHamburgerMenu();
			}
		},

		{
			id: 3,
			label: 'Contact Us',
			href: '#footer',
			onclick: () => {
				CloseHamburgerMenu();
			}
		},
		{
			id: 4,
			label: 'Close',
			href: '#',
			onclick: () => {
				CloseHamburgerMenu()
			}
		}
	];

	let hamburgerMenuIsOpen = false;
	function CloseHamburgerMenu() {
		hamburgerMenuIsOpen = false;
		const html = document.querySelector('html');
		if (html) {
			if (hamburgerMenuIsOpen) {
				html.classList.add('overflow-hidden');
			} else {
				html.classList.remove('overflow-hidden');
			}
		}
	}

	function ToggleHamburgerMenu() {
		hamburgerMenuIsOpen = !hamburgerMenuIsOpen;
		const html = document.querySelector('html');
		if (html) {
			if (hamburgerMenuIsOpen) {
				html.classList.add('overflow-hidden');
			} else {
				html.classList.remove('overflow-hidden');
			}
		}
	}
	function toggleOverflowHidden(node: HTMLElement) {
		node.addEventListener('click', ToggleHamburgerMenu);
	}
	let innerWidth = 0;
</script>

<svelte:window bind:innerWidth />
<header
	class="fixed left-0 top-0 z-50 w-full -translate-y-4 animate-fade-in border-b opacity-0 backdrop-blur-md"
>
	<!-- {#if innerWidth < 768} -->
	<div class="container flex h-14 items-center justify-between">
		<ARE_Logo
			target="_blank"
			href="https://ensi.rnu.tn/Anniv40ENSI/Clubs/ARE/index.html"
			style="width: 50px;height: 50px"
		/>

		<div class="ml-auto flex h-full items-center">
			<div class="hidden h-full items-center sm:flex">
				<LightSwitch class="mr-4" />
				<a class="mr-6 text-sm" href="/livescores">Live Scores</a>
			</div>
			<Button variant="default" class="text-sm" href="/register">Register</Button>
		</div>
		<button class="ml-6 md:hidden" use:toggleOverflowHidden>
			<span class="sr-only">Toggle menu</span>
			{#if hamburgerMenuIsOpen}
				<XIcon strokeWidth={1.4} class="text-gray-300" />
			{:else}
				<AlignJustify strokeWidth={1.4} class="text-gray-300" />
			{/if}
		</button>
	</div>
	<!-- {/if} -->
</header>

<nav
	class={cn(
		`fixed left-0 top-0 z-50 h-screen w-full overflow-auto `,
		{
			'pointer-events-none': !hamburgerMenuIsOpen
		},
		{
			'bg-background/70 backdrop-blur-md': hamburgerMenuIsOpen
		}
	)}
>
	{#if hamburgerMenuIsOpen === true}
		<div class="container flex h-14 items-center justify-between">
			<a class="text-md flex items-center" href="#"> ARE</a>

			<button class="md:hidden" use:toggleOverflowHidden>
				<span class="sr-only">Toggle menu</span>
				{#if hamburgerMenuIsOpen}
					<XIcon strokeWidth={1.4} class="text-gray-300" />
				{:else}
					<AlignJustify strokeWidth={1.4} class="text-gray-300" />
				{/if}
			</button>
		</div>
		<ul
			in:fly={{ y: -30, duration: 400 }}
			class="flex flex-col uppercase ease-in md:flex-row md:items-center md:normal-case"
		>
			{#each menuItem as item, i}
				<li class="border-grey-dark border-b py-0.5 pl-6 md:border-none">
					<a
						class="hover:text-grey flex h-[var(--navigation-height)] w-full items-center text-xl transition-[color,transform] duration-300 md:translate-y-0 md:text-sm md:transition-colors {hamburgerMenuIsOpen
							? '[&_a]:translate-y-0'
							: ''}"
						href={item.href}
						on:click={item.onclick ? item.onclick : () => {}}
					>
						{item.label}
					</a>
				</li>
			{/each}
		</ul>
	{/if}
</nav>
