<script lang="ts">
	import { goto } from '$app/navigation';
	import { client } from '$lib/auth-client';

	const session = client.useSession();

	if (!session) {
		goto('/login');
	}
</script>

<div class="h-screen w-screen items-center justify-center">
	<div>
		<p>Welcome {$session?.data?.user?.name}</p>
		<button
			onclick={() =>
				client.signOut({
					fetchOptions: {
						onSuccess: () => goto('/login')
					}
				})}>Sign Out</button
		>
	</div>
</div>
