<script>
	
	let username = "";	
	let email = "";
	let password = "";
	let password2 = "";

	// Show input error message
	function showError(id, message) {		
		let elem = document.getElementById(id).parentElement;		
  		elem.className = 'form-control error';
  		const small = elem.querySelector('small');
  		small.innerText = message;
	}

	// Show success outline
	function showSuccess(id) {		
		let elem = document.getElementById(id).parentElement;		
  		elem.className = 'form-control success';
	}

	// Check email is valid
	function checkEmail(input) {
		const re =
			/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
		if (re.test(input.trim())) {
			showSuccess("email");
		} else {
			showError("email", "Email is not valid");
		}
	}
	
	// Check input length
	function checkLength(name, input, min, max) {
		if (input.length < min) {
			showError(
				name,
				`${name} must be at least ${min} characters`
			);
		} else if (input.length > max) {
			showError(
				name,
				`${name} must be less than ${max} characters`
			);
		} else {
			showSuccess(name);
		}
	}

	// Check passwords match
	function checkPasswordsMatch(input1, input2) {
		if (input1 !== input2) {
			showError("password2", "Passwords do not match");
		}
	}

	// Event listeners	
	function handleSubmit(e) {
		e.preventDefault();
		checkLength("username", username, 3, 15);
		console.log("password: ", password);
		checkLength("password", password, 6, 25);
		checkEmail(email);
		checkPasswordsMatch(password, password2);
	};
</script>

<main>
	<div class="container">
		<form id="form" class="form">
			<h2>Register</h2>
			<div class="form-control">
				<label for="username">Username</label>
				<input type="text" bind:value={username} id="username" placeholder="Enter username" required>
				<small></small>
			</div>
			<div class="form-control">
				<label for="email">Email</label>
				<input type="text" bind:value={email} id="email" placeholder="Enter email" required>
				<small></small>
			</div>
			<div class="form-control">
				<label for="password">Password</label>
				<input
					type="password"
					id="password"
					bind:value={password}
					placeholder="Enter password"
					required
				/>
				<small></small>
			</div>
			<div class="form-control">
				<label for="password2">Confirm Password</label>
				<input
					type="password"
					id="password2"
					bind:value={password2}
					placeholder="Enter password again"
					required
				/>
				<small></small>
			</div>
			<button on:submit={handleSubmit} type="submit">Submit</button>
		</form>
	</div>
</main>

<style>
</style>
