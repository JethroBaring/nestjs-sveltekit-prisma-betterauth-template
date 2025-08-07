<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import { Label } from '$lib/components/ui/label/index.js';
	import { Input } from '$lib/components/ui/input/index.js';
	import { cn } from '$lib/utils.js';
	import type { HTMLAttributes } from 'svelte/elements';
	import { signIn } from '$lib/auth-client';

	let { class: className, ...restProps }: HTMLAttributes<HTMLDivElement> = $props();

	const id = $props.id();

	let email = $state('');
	let password = $state('');
	let loading = $state(false);

	const handleSignIn = async () => {
		loading = true;
		await signIn.email(
			{
				email: email,
				password: password,
				callbackURL: '/dashboard'
			},
			{
				onError(context) {
					alert(context.error.message);
				}
			}
		);
		loading = false;
	};
</script>

<div class={cn('flex flex-col gap-6', className)} {...restProps}>
	<Card.Root>
		<Card.Header class="text-center">
			<Card.Title class="text-xl">Welcome back</Card.Title>
			<Card.Description>Login with your account</Card.Description>
		</Card.Header>
		<Card.Content>
			<form>
				<div class="grid gap-6">
					<div class="grid gap-6">
						<div class="grid gap-3">
							<Label for="email-{id}">Email</Label>
							<Input id="email-{id}" type="email" placeholder="m@example.com" required bind:value={email} />
						</div>
						<div class="grid gap-3">
							<div class="flex items-center">
								<Label for="password-{id}">Password</Label>
								<a href="##" class="ml-auto text-sm underline-offset-4 hover:underline">
									Forgot your password?
								</a>
							</div>
							<Input id="password-{id}" type="password" required bind:value={password} />
						</div>
						<Button class="w-full" onclick={handleSignIn} disabled={loading}>
							{loading ? 'Signing in...' : 'Sign In'}
						</Button>
					</div>
					<div class="text-center text-sm">
						Don&apos;t have an account?
						<a href="/signup" class="underline underline-offset-4"> Sign up </a>
					</div>
				</div>
			</form>
		</Card.Content>
	</Card.Root>
	<div
		class="text-muted-foreground *:[a]:hover:text-primary *:[a]:underline *:[a]:underline-offset-4 text-balance text-center text-xs"
	>
		By clicking continue, you agree to our <a href="##">Terms of Service</a>
		and <a href="##">Privacy Policy</a>.
	</div>
</div>
