<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import { Label } from '$lib/components/ui/label/index.js';
	import { Input } from '$lib/components/ui/input/index.js';
	import { cn } from '$lib/utils.js';
	import type { HTMLAttributes } from 'svelte/elements';
	import { signUp } from '$lib/auth-client';
	import { goto } from '$app/navigation';

	let { class: className, ...restProps }: HTMLAttributes<HTMLDivElement> = $props();

	const id = $props.id();

	let firstName = $state('');
	let lastName = $state('');
	let email = $state('');
	let password = $state('');
	let loading = $state(false);

	const handleSignUp = async () => {
		loading = true;
		const user = {
			firstName: firstName,
			lastName: lastName,
			email: email,
			password: password
		};
		await signUp.email({
			email: user.email,
			password: user.password,
			name: `${user.firstName} ${user.lastName}`,
			callbackURL: '/',
			fetchOptions: {
				onSuccess() {
					alert('Your account has been created.');
					goto('/dashboard');
				},
				onError(context) {
					alert(context.error.message);
				}
			}
		});
		loading = false;
	};
</script>

<div class={cn('flex flex-col gap-6', className)} {...restProps}>
	<Card.Root>
		<Card.Header class="text-center">
			<Card.Title class="text-xl">Welcome</Card.Title>
			<Card.Description>Create an account</Card.Description>
		</Card.Header>
		<Card.Content>
			<form>
				<div class="grid gap-6">
					<div class="grid gap-6">
						<div class="grid gap-3">
							<Label for="first-name-{id}">First Name</Label>
							<Input id="first-name-{id}" type="text" placeholder="John" required bind:value={firstName} />
						</div>
						<div class="grid gap-3">
							<Label for="last-name-{id}">Last Name</Label>
							<Input id="last-name-{id}" type="text" placeholder="Doe" required bind:value={lastName} />
						</div>
						<div class="grid gap-3">
							<Label for="email-{id}">Email</Label>
							<Input id="email-{id}" type="email" placeholder="m@example.com" required bind:value={email} />
						</div>
						<div class="grid gap-3">
							<Label for="password-{id}">Password</Label>
							<Input id="password-{id}" type="password" required bind:value={password} />
						</div>
						<Button class="w-full" onclick={handleSignUp} disabled={loading}>
							{loading ? 'Signing up...' : 'Sign Up'}
						</Button>
					</div>
					<div class="text-center text-sm">
						Already have an account?
						<a href="/signin" class="underline underline-offset-4"> Sign In </a>
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
