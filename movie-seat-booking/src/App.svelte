<script>
	import { onMount } from "svelte";
	
	let moviePrice = 0;
	let movieIndex = 0;
	let count = 0; 
	let total = 0;

	function allSeats() {
		return document.querySelectorAll('.row .seat:not(.occupied)');
	}

	// Update total and count
	function updateSelected() {
		const selectedSeats = document.querySelectorAll(".row .seat.selected");

		const seatsIndex = [...selectedSeats].map((seat) =>
			[...allSeats()].indexOf(seat)
		);

		localStorage.setItem("selectedSeats", JSON.stringify(seatsIndex));

		const selectedSeatsCount = selectedSeats.length;
		count = selectedSeatsCount;
		total = selectedSeatsCount * moviePrice;		
	}

	// Get data from localstorage and populate UI
	function populateUI() {
		const selectedSeats = JSON.parse(localStorage.getItem("selectedSeats"));

		let num = 0;
		if (selectedSeats !== null && selectedSeats.length > 0) {
			allSeats().forEach((seat, index) => {
				if (selectedSeats.indexOf(index) > -1) {
					seat.classList.add("selected");
					num += 1;
				}
			});
		}
		count = num;

		const selectedMovieIndex = localStorage.getItem("selectedMovieIndex");

		if (selectedMovieIndex !== null) {
			document.getElementById("movie").selectedIndex = selectedMovieIndex;
			movieIndex = selectedMovieIndex;
		}
		moviePrice = document.getElementById("movie").value;
		total = count * moviePrice;	

		// Seat click event
		const container = document.querySelector('.container');
		container.addEventListener("click", (e) => {
			if (
				e.target.classList.contains("seat") &&
				!e.target.classList.contains("occupied")
			) {
				e.target.classList.toggle("selected");

				updateSelected();
			}
		});
	}

	function handleMovieSelect(e) {
		movieIndex = e.target.selectedIndex;
		moviePrice = +e.target.value;
		total = count * moviePrice;
		localStorage.setItem("selectedMovieIndex", movieIndex);
		localStorage.setItem("selectedMoviePrice", moviePrice);
	}

	onMount(() => {		
		populateUI();		
	});
</script>

<main>
	<div class="movie-container">
		<span>Pick a movie:</span>
		<select id="movie" on:change={handleMovieSelect}>
			<option value="10">Avengers: Endgame ($10)</option>
			<option value="12">Joker ($12)</option>
			<option value="8">Toy Story 4 ($8)</option>
			<option value="9">The Lion King ($9)</option>
		</select>
	</div>

	<ul class="showcase">
		<li>
			<div class="seat" />
			<small>N/A</small>
		</li>
		<li>
			<div class="seat selected" />
			<small>Selected</small>
		</li>
		<li>
			<div class="seat occupied" />
			<small>Occupied</small>
		</li>
	</ul>

	<div class="container">
		<div class="screen" />

		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
		</div>
		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat occupied" />
			<div class="seat occupied" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
		</div>
		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat occupied" />
			<div class="seat occupied" />
		</div>
		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
		</div>
		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat occupied" />
			<div class="seat occupied" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
		</div>
		<div class="row">
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat" />
			<div class="seat occupied" />
			<div class="seat occupied" />
			<div class="seat occupied" />
			<div class="seat" />
		</div>
	</div>

	<p class="text">
		You have selected <span id="count">{count}</span> seats for a price of $<span id="total">${total}</span>
	</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
