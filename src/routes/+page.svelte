<script>
	import Tabs from '/src/lib/tabs/Tabs.svelte';
	import Home from './Home.svelte';
	import About from './About.svelte';
	import Settings from './Settings.svelte';

	const tabs = [
		{ name: 'Tab-1', content: Home },
		{ name: 'Tab-2', content: About },
		{ name: 'Tab-3', content: Settings },
		{ name: 'Tab-4', content: Home },
		{ name: 'Tab-5', content: About },
		{ name: 'Tab-6', content: Settings },
		{ name: 'Tab-7', content: Home },
		{ name: 'Tab-8', content: About },
		{ name: 'Tab-9', content: Settings },
		{ name: 'Tab-10', content: Home },
		{ name: 'Tab-11', content: About },
		{ name: 'Tab-12', content: Settings },
		{ name: 'Tab-13', content: Home },
		{ name: 'Tab-14', content: About },
		{ name: 'Tab-15', content: Settings },
	];
</script>

<Tabs {tabs} />
