<script context="module" lang="ts">
	export type ToggleButtonContext = {
		selected: Writable<any>
		select: (value: any) => void
	}
</script>

<script lang="ts">
	import { createEventDispatcher, setContext } from 'svelte'
	import { writable, type Writable } from 'svelte/store'
	import { TabGroup, TabList } from '@rgossiaux/svelte-headlessui'
	import { twMerge } from 'tailwind-merge'

	export let id: string | null | undefined = undefined
	export let selected: any
	export let noWFull: boolean = false
	export let disabled: boolean = false
	export let tabListClass: string = ''

	const dispatch = createEventDispatcher()
	const selectedContent = writable(selected)

	function setSelected(selected: any) {
		if ($selectedContent != selected) {
			selectedContent.set(selected)
		}
	}

	$: setSelected(selected)

	setContext<ToggleButtonContext>('ToggleButtonGroup', {
		selected: selectedContent,
		select: (value: any) => {
			selectedContent.set(value)
			selected = value
			dispatch('selected', value)
		}
	})
</script>

<TabGroup
	{id}
	class={twMerge(
		`h-8 flex ${noWFull ? '' : 'w-full'} ${disabled ? 'disabled' : ''}`,
		$$props.class
	)}
>
	<TabList
		class={twMerge('flex bg-surface-secondary rounded-md p-0.5 gap-1 h-full ', tabListClass)}
	>
		<slot />
	</TabList>
</TabGroup>
