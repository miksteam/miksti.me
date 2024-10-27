<script lang="ts">
	import { onMount } from 'svelte';
	import fall from './audiodelay.assets/fall.mp3';
	import pipePng from './audiodelay.assets/pipe.png';
	import pipeWebp from './audiodelay.assets/pipe.webp';
	import audiodelayPng from '../../Feed/Feed.assets/audiodelay.png';

	let audioContext: AudioContext;
	let latency: string = 'click on pipe to evaluate';
	let hasLatencySupport: boolean | undefined = undefined;
	let image: HTMLImageElement;
	let volumeInput: HTMLInputElement;
	let lastVolume = 1;

	let audio: HTMLAudioElement;
	let readyToPlay = false;
	let shouldStart = false;
	let started = false;
	let isMuted = false;

	const play = () => {
		if (!audioContext) {
			audioContext = new AudioContext();
		}
		if (!readyToPlay) {
			audio.load();
		}
		shouldStart = true;
	};

	const onCanPLayThrough = () => {
		readyToPlay = true;
	};

	const onVolumeInput = () => {
		lastVolume = Number(volumeInput.value);
		audio.volume = lastVolume;
		isMuted = lastVolume === 0;
	};

	const onMuteToggle = () => {
		if (audio.volume === 0) {
			audio.volume = lastVolume;
			volumeInput.value = String(lastVolume);
			if (lastVolume > 0) {
				isMuted = false;
			}
		} else {
			audio.volume = 0;
			volumeInput.value = '0';
			isMuted = true;
		}
	};

	const onKeydown = (e: KeyboardEvent) => {
		if (e.key === 'm' && !e.shiftKey && !e.metaKey && !e.altKey) {
			onMuteToggle();
		}
	};

	$: if (readyToPlay && shouldStart) {
		shouldStart = false;
		audio?.play();
		audio.currentTime = 0;
		if (hasLatencySupport) {
			latency = audioContext.outputLatency.toFixed(3) + 's';
		}

		// trigger reflow to restart animation
		image.style.animation = 'none';
		image.offsetHeight;
		image.style.animation = '';
		started = true;
	}

	onMount(() => {
		hasLatencySupport = 'outputLatency' in AudioContext.prototype;
	});
</script>
<svelte:head>
	<title>Audio Delay</title>
	<meta name="description" content="Collection of articles" />
	<meta property="og:title" content="Clipboard API" />
	<meta property="og:type" content="article" />
	<meta property="og:description" content="Challenges with AudioContext.outputLatency in Web Audio" />
	<meta property="og:image" content={audiodelayPng} />
	<meta property="og:url" content="https://miksti.me/demo/clipboard" />
	<meta name="twitter:card" content={audiodelayPng} />
	<meta name="keywords" content="Audio Delay, Michael Balitsky, Михаил Балицкий, mikstime" />
</svelte:head>
<svelte:window on:keydown={onKeydown} />
<h1>Audio Delay</h1>
<p>
	AudioContext.outputLatency is a property in the Web Audio API that provides an estimate of the audio output latency of an AudioContext. This represents the delay between when an audio buffer is scheduled for playback and when it is actually heard through the speakers.
</p>
<h2>Inconsistent Support</h2>
<p>
	The delay returned from AudioContext.outputLatency is likely to be zero for the first few seconds of a playback. Latency varies
	depending on the platform and the available hardware.
</p>
<p>
	The output latency can change due to various factors, like system load or audio processing needs,
	making it challenging to predict and compensate for latency accurately.
</p>
<p>
	It provides a general estimate of latency, which might not be granular enough for
	applications requiring extremely tight timing control.
</p>
<h2>Demo </h2>
<p>
	If you are using wireless headphones or speakers to play this demo you might experience a delay between audio and animation
	during first playthrough.
</p>
<hr />
<section class="demo">
	<audio bind:this={audio} on:canplaythrough={onCanPLayThrough} preload="auto">
		<source src={fall} />
	</audio>
	<div class="volume">
		<input
			class="volume__range"
			aria-label="Volume level"
			type="range"
			max="1"
			value="1"
			step="0.01"
			bind:this={volumeInput}
			on:input={onVolumeInput}
		/>
		<button
			class="volume__button"
			class:volume__button_muted_yes={isMuted}
			aria-keyshortcuts="m"
			aria-label={isMuted ? 'Unmute' : 'Mute'}
			on:click={onMuteToggle}
		/>
	</div>
	<button on:click={play} class:pipe_fall_yes={started} class="pipe">
		<picture>
			<source type="image/webp" srcset={pipeWebp} />
			<img src={pipePng} alt="Metal pipe" class="pipe__image" bind:this={image} />
		</picture>
	</button>
</section>
<output class="latency-preview">Current Latency: <b>{latency}</b></output>
{#if !hasLatencySupport}
	<div class="callout warning">
		<p class="callout__paragraph">
			This demo is not supported by your browser. Try using different browser (<a
				class="link"
				target="_blank"
				rel="noopener noreferrer"
				href="https://caniuse.com/mdn-api_audiocontext_outputlatency">supported browsers</a
			>).
		</p>
	</div>
{/if}

<style>
	.pipe {
		background: none;
		outline: none;
		border: none;
		display: block;
		margin: auto;
		padding-bottom: 100px;
	}

	.pipe_fall_yes .pipe__image {
		transform: scale(0.98) translateY(100px);

		animation: fall 3s linear;
	}

	.pipe__image {
		pointer-events: none;
		width: 100%;
	}

	.volume {
		display: flex;
		margin: auto;
		justify-content: center;
		padding: 16px;
	}
	.volume__range {
		-webkit-appearance: none;
		appearance: none;
		background: transparent;
		cursor: pointer;
		flex: 1;
		max-width: 200px;
	}

	.volume__range::-webkit-slider-runnable-track {
		background: rgba(255, 255, 255, 0.2);
		height: 8px;
		border-radius: 8px;
	}

	.volume__range::-moz-range-track {
		background: rgba(255, 255, 255, 0.2);
	}

	.volume__range::-webkit-slider-thumb {
		-webkit-appearance: none;
		appearance: none;
		margin-top: -4px;
		background-color: #c1c1c1;
		height: 16px;
		width: 16px;
		border-radius: 16px;
	}

	.volume__range:active::-webkit-slider-thumb {
		background-color: #a9a9a9;
	}

	.volume__button {
		background: none;
		border: none;
		outline: none;
		background: url($lib/images/mute.svg) center;
		width: 44px;
		height: 44px;
		padding: 0;
	}

	.volume__button_muted_yes {
		background-image: url($lib/images/unmute.svg);
	}

	@keyframes fall {
		0% {
			transform: scale(1) translateY(0);
		}
		7% {
			transform: scale(1.1) translateY(100px);
		}
		23% {
			transform: scale(1.05) translateY(90px);
		}
		25% {
			transform: scale(1.08) translateY(100px);
		}
		28% {
			transform: scale(1.02) translateY(95px);
		}
		30% {
			transform: scale(1) translateY(100px);
		}
		35% {
			transform: scale(1.05) translateY(90px);
		}
		40% {
			transform: scale(1.08) translateY(100px);
		}
		43% {
			transform: scale(1.02) translateY(95px);
		}
		45% {
			transform: scale(1) translateY(100px);
		}
		50% {
			transform: scale(1.02) translateY(100px);
		}
		60% {
			transform: scale(1) translateY(100px);
		}
		75% {
			transform: scale(1.01) translateY(100px);
		}
		100% {
			transform: scale(0.98) translateY(100px);
		}
	}
</style>
