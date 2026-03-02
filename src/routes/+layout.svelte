<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { createSvelteAuthClient } from '@mmailaender/convex-better-auth-svelte/svelte';
	import { authClient } from '$lib/auth-client';
	import { useAuth } from '@mmailaender/convex-better-auth-svelte/svelte';
	import Inspect from 'svelte-inspect-value';
	import { useQuery } from 'convex-svelte';
	import { api } from '$convex/_generated/api';
    import type { LayoutProps } from './$types';

    let { data, children }: LayoutProps = $props();

	createSvelteAuthClient({ authClient, getServerState: () => data.authState });

	const auth = useAuth();

	const userIdenity = useQuery(api.auth.getUserIdentity, () => ({}));
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<Inspect name="auth" value={{
	'auth.isAuthenticated': auth.isAuthenticated,
	'auth.isLoading': auth.isLoading,
}} />
<Inspect name="userIdenity" value={userIdenity.data} />
<Inspect name="authState" value={data.authState} />


<main>
	{@render children?.()}
</main>
