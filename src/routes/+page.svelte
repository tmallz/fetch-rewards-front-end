<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import * as yup from 'yup';

	let stateArray = [];
	let occupationArray = [];
	let values = {
		name: '',
		email: '',
		password: '',
		occupation: '',
		state: '',
	};
	let errors = {};

	const schema = yup.object().shape({
		name: yup.string().required('Name is required'),
		email: yup.string().required('Email is required').email('Email is invalid'),
		password: yup.string().required('Password is required'),
		occupation: yup.string().ensure().required('Select an occupation'),
		state: yup.string().ensure().required('Select a state'),
	});

	onMount(async () => {
		try {
			await fetch('https://frontend-take-home.fetchrewards.com/form').then(
				res => {
					if (res.ok) {
						res.json().then(data => {
							occupationArray = data.occupations;
							stateArray = data.states;
						});
					}
				}
			);
		} catch (err) {
			console.error(err);
		}
	});

	var handleSignUpButton = async () => {
		try {
			await schema.validate(values, { abortEarly: false });
			await handlePost();
			errors = {};
		} catch (err) {
			errors = err.inner.reduce((acc, err) => {
				return { ...acc, [err.path]: err.message };
			}, {});
		}
	};

	const handlePost = async () => {
		try {
			await fetch('https://frontend-take-home.fetchrewards.com/form', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
				},
				mode: 'cors',
				credentials: 'same-origin',
				body: JSON.stringify({
					name: values.name,
					email: values.email,
					password: values.password,
					occupation: values.occupation,
					state: values.state,
				}),
			})
				.then(json => console.log(json))
				.then(goto('/success'));
		} catch (err) {
			console.error(err);
		}
	};
</script>

<div class="hero min-h-screen bg-base-200">
	<div class="hero-content max-w-xl flex-col lg:flex-row-reverse">
		<div class="card flex-shrink-0 w-full max-w-lg shadow-2xl bg-base-100">
			<div class="card-body">
				<div class="form-control">
					<label class="label" for="name">
						<span class="label-text">Name</span>
					</label>
					<input
						type="text"
						placeholder="First and Last Name"
						class="input input-bordered"
						bind:value={values.name}
					/>
					{#if errors.name}
						<span class="text-error">{errors.name}</span>
					{/if}
				</div>
				<div class="form-control">
					<label class="label" for="email">
						<span class="label-text">Email</span>
					</label>
					<input
						type="text"
						placeholder="email"
						class="input input-bordered"
						bind:value={values.email}
					/>
					{#if errors.email}
						<span class="text-error">{errors.email}</span>
					{/if}
				</div>
				<div class="form-control">
					<label class="label" for="password">
						<span class="label-text">Password</span>
					</label>
					<input
						type="password"
						placeholder="password"
						class="input input-bordered"
						bind:value={values.password}
					/>
					{#if errors.password}
						<span class="text-error">{errors.password}</span>
					{/if}
					<div class="form-control">
						<label class="label" for="email">
							<span class="label-text">Occupation</span>
						</label>
						<select
							class="select w-full max-w-xs input-bordered text-gray-400 font-normal text-lg"
							bind:value={values.occupation}
						>
							<option disabled selected>occupation</option>
							{#each occupationArray as occupation}
								<option class="selected:text-black">{occupation}</option>
							{/each}
						</select>
						{#if errors.occupation}
							<span class="text-error">{errors.occupation}</span>
						{/if}
					</div>
					<div class="form-control">
						<label class="label" for="email">
							<span class="label-text">State</span>
						</label>
						<select
							class="select w-full max-w-xs input-bordered text-gray-400 font-normal text-lg"
							bind:value={values.state}
						>
							<option class disabled selected>state</option>
							{#each stateArray as state}
								<option>{state.abbreviation}</option>
							{/each}
						</select>
						{#if errors.state}
							<span class="text-error">{errors.state}</span>
						{/if}
					</div>
				</div>
				<div class="form-control mt-6">
					<button
						class="btn btn-primary"
						on:click|preventDefault={handleSignUpButton}>Sign Up</button
					>
				</div>
			</div>
		</div>
	</div>
</div>
