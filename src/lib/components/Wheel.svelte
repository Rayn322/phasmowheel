<script lang="ts">
	import { scaleForHighRes } from '$lib/util/canvas';

	let canvas: HTMLCanvasElement;
	let ctx: CanvasRenderingContext2D;
	let reqId: number;
	let options = ['Wraith', 'Mare', 'Demon', 'Shade', 'Jinn', 'Oni'];

	$effect(() => {
		const gottenCtx = canvas.getContext('2d');

		if (gottenCtx) {
			ctx = gottenCtx;
		} else {
			throw new Error('2d context not supported');
		}
		scaleForHighRes(canvas, ctx);
		ctx.lineWidth = 2;

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

		ctx.translate(250, 250);
		ctx.font = '30px Inter';

		// filled arcs
		for (let i = 0; i < options.length; i++) {
			ctx.save();

			// idk copilot did these colors
			ctx.fillStyle = 'hsl(' + (i * 360) / options.length + ', 100%, 50%)';

			ctx.beginPath();
			ctx.moveTo(0, 0);
			ctx.arc(
				0,
				0,
				245,
				((2 * Math.PI) / options.length) * i,
				((2 * Math.PI) / options.length) * (i + 1)
			);
			ctx.fill();

			ctx.restore();
		}

		// divider lines
		for (let i = 0; i < options.length; i++) {
			ctx.save();

			ctx.beginPath();
			ctx.moveTo(0, 0);
			ctx.rotate(((2 * Math.PI) / options.length) * i);
			ctx.lineTo(245, 0);
			ctx.stroke();

			ctx.restore();
		}

		ctx.beginPath();
		ctx.arc(0, 0, 245, 0, 2 * Math.PI, false);
		ctx.stroke();

		// text
		for (let i = 0; i < options.length; i++) {
			ctx.save();

			// TODO: make the math not look stupid (i speedran this with copilot)

			const measurements = ctx.measureText(options[i]);
			const textWidth = measurements.width;

			// rotate to divider line and then rotate to middle of section
			ctx.rotate(((2 * Math.PI) / options.length) * i + Math.PI / options.length);
			ctx.fillText(options[i], 225 - textWidth, measurements.actualBoundingBoxAscent / 2);

			ctx.restore();
		}

		ctx.translate(-250, -250);

		reqId = requestAnimationFrame(animate);
	}
</script>

<canvas width="500" height="500" bind:this={canvas}>A spinny wheel</canvas>
