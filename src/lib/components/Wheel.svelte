<script lang="ts">
	import { scaleForHighRes } from '$lib/util/canvas';

	let canvas: HTMLCanvasElement;
	let ctx: CanvasRenderingContext2D;
	let reqId: number;

	$effect(() => {
		const gottenCtx = canvas.getContext('2d');

		if (gottenCtx) {
			ctx = gottenCtx;
		} else {
			throw new Error('2d context not supported');
		}

		scaleForHighRes(canvas, ctx);

		let f = new FontFace(
			'Inter',
			'url(https://rsms.me/inter/font-files/InterVariable.woff2?v=4.0)'
		);

		f.load().then(() => {
			reqId = requestAnimationFrame(animate);
		});

		return () => {
			cancelAnimationFrame(reqId);
		};
	});

	function animate(timestamp: number) {
		ctx.clearRect(0, 0, canvas.width, canvas.height);

		ctx.beginPath();
		ctx.arc(250, 250, 245, 0, 90, false);
		ctx.stroke();

		ctx.beginPath();
		ctx.arc(250, 250, 5, 0, 360, false);
		ctx.fill();

		ctx.font = '40px Inter';
		ctx.fillText('Hello, world!', 250, 250);

		ctx.translate(250, 250);
		ctx.rotate(timestamp / 1000);
		ctx.font = '30px Inter';

		const measured = ctx.measureText(timestamp.toString());
		console.log(measured);

		ctx.fillText(timestamp.toString(), -measured.width / 2, measured.actualBoundingBoxAscent / 2);
		ctx.rotate(-timestamp / 1000);

		ctx.translate(-250, -250);

		reqId = requestAnimationFrame(animate);
	}
</script>

<canvas width="500" height="500" bind:this={canvas}>A spinny wheel</canvas>
