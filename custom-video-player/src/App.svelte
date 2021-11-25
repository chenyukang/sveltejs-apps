<script>
	import { onMount } from "svelte";

	let video = null;
	let play = null;
	let stop = null;
	let progress = null;
	let timestamp = null;
	let playHtml = '<i class="fa fa-play fa-2x"></i>';

	// Play & pause video
	function toggleVideoStatus() {
		if (video.paused) {
			video.play();
		} else {
			video.pause();
		}
	}

	// update play/pause icon
	function updatePlayIcon() {
		if (video.paused) {
			playHtml = '<i class="fa fa-play fa-2x"></i>';
		} else {
			playHtml = '<i class="fa fa-pause fa-2x"></i>';
		}
	}

	// Update progress & timestamp
	function updateProgress() {
		progress.value = (video.currentTime / video.duration) * 100;

		// Get the minutes
		let mins = Math.floor(video.currentTime / 60);
		if (mins < video.duration) {
			mins = "0" + String(mins);
		}

		// Get Seconds
		let secs = Math.floor(video.currentTime % 60);
		if (secs < video.duration) {
			secs = "0" + String(secs);
		}

		timestamp.innerHTML = `${mins}:${secs}`;
	}

	// Set video time to progress
	function setVideoProgress() {
		video.currentTime = (+progress.value * video.duration) / 100;
	}

	// Stop video
	function stopVideo() {
		video.currentTime = 0;
		video.pause();
	}

	onMount(() => {
		video = document.getElementById("video");
		play = document.getElementById("play");
		stop = document.getElementById("stop");
		progress = document.getElementById("progress");
		timestamp = document.getElementById("timestamp");
	});
</script>

<h1>Custom Video Player</h1>
<video
	on:click={toggleVideoStatus}
	on:play={updatePlayIcon}
	on:pause={updatePlayIcon}
	on:timeupdate={updateProgress}
	bind:innerHTML={playHtml}
	src="gone.mp4"
	id="video"
	class="screen"
	poster="poster.png"
/>
<div class="controls">
	<button class="btn" id="play" on:click={toggleVideoStatus}>
		<i class="fa fa-play fa-2x" />
	</button>
	<button class="btn" id="stop" on:click={stopVideo}>
		<i class="fa fa-stop fa-2x" />
	</button>
	<input
		type="range"
		id="progress"
		class="progress"
		min="0"
		max="100"
		step="0.1"
		value="0"
		on:change={setVideoProgress}
	/>
	<span class="timestamp" id="timestamp">00:00</span>
</div>
