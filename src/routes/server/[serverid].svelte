<script>
	import { onDestroy, onMount } from 'svelte';
	import io from 'socket.io-client';
	import VolumeCanvas from '../../components/VolumeCanvas.svelte';
	import { page } from '$app/stores';

	let clientCount = 0;
	let serverName = $page.params.serverid;
    let emitRate = $page.query.get('emitrate') || 250;
	let audioCtx, analyser, source, volumeData, socket, emitInterval;

    onDestroy(() => {
        if(emitInterval) {
            clearInterval(emitInterval)
        }
    })

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

		// Set up socket in browser only
		socket = io('http://localhost:5000', {
			query: { serverName: $page.params.serverid, socketType: 'server' }
		});

		socket.on('clientcount', (updatedClientCount) => {
			clientCount = updatedClientCount;
		});
	});

	function processStream() {
		let updateAudioData = function () {
			requestAnimationFrame(updateAudioData);

			analyser.getByteFrequencyData(volumeData);
		};

        updateAudioData();

        emitInterval = setInterval(() => {
			if (clientCount > 0) {
				emitAudioData();
			}
        }, emitRate)

	}

	function emitAudioData() {
		socket.emit('volumedata', {
			serverid: serverName,
			volumeData: volumeData
		});
	}
</script>

<h2>{serverName}</h2>
<p>Clients listening: {clientCount}</p>
<VolumeCanvas {volumeData} />
