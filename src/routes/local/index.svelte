<script>
	import { onMount } from 'svelte';
	import VolumeCanvas from '../../components/VolumeCanvas.svelte';

	let audioCtx, analyser, source, volumeData;

	onMount(() => {
		audioCtx = new (window.AudioContext || window.webkitAudioContext)();
		audioCtx.resume();
		analyser = audioCtx.createAnalyser();

		analyser.fftSize = 2048;
		analyser.minDecibels = -90;
		analyser.maxDecibels = 0;
		analyser.smoothingTimeConstant = 0.85;

		volumeData = new Uint8Array(analyser.frequencyBinCount);

		navigator.mediaDevices
			.getUserMedia({ audio: true })
			.then((stream) => {
				source = audioCtx.createMediaStreamSource(stream);
				source.connect(analyser);
				processStream();
			})
			.catch((err) => {
				console.log('The following gUM error occured: ' + err);
			});
	});

	function processStream() {
		let updateAudioData = function () {
			requestAnimationFrame(updateAudioData);

			analyser.getByteFrequencyData(volumeData);
		};

		updateAudioData();
	}
</script>

<h2>Your Microphone</h2>
<p>This is only local. No data is being sent to the server.</p>
<VolumeCanvas {volumeData} />
