/*==========================================================================
	template
==========================================================================*/
<template>
  <div id="app">
		<h1>vue-svg-distotion</h1>
		<p><a
			href="https://github.com/yoshihitofujiwara/vue-svg-distotion" target="_blank">
			<SvgDistotion class="svg"
				@mouseleave="onPassive"
				@mouseenter="onActive"
				ref="svg"
				:image="image"
				:filter="filter"
				:duration="duration"
				:ease="ease"
			/>
		</a></p>
  </div>
</template>


/*==========================================================================
	script
==========================================================================*/
<script>
import dat from "dat.gui";
import SvgDistotion from './components/SvgDistotion.vue'

let gui;


export default {
  name: 'app',

  components: {
    SvgDistotion
  },

	data(){
		return {
			image: {
				passive: "./assets/img/img01.jpg",
				active: "./assets/img/img02.jpg",
				width: "90%",
				height: "90%",
				x: "5%",
				y: "5%"
			},
			filter: {
				type: "fractalNoise",
				baseFrequencyX: 0.01,
				baseFrequencyY: 0.003,
				numOctaves: 2,
				seed: 2,
				stitchTiles: "noStitch",
				scale: 80,
				maxScale: 100
			},
			duration: 0.8,
			ease: "Power1.easeOut"
		}
	},

	mounted(){
		gui = new dat.GUI({ autoPlace: false });
		gui.domElement.style.position = "fixed";
		gui.domElement.style.top = "0";
		gui.domElement.style.right = "0";
		document.body.appendChild(gui.domElement);

		let params = {
			size: 90,
			x: 5,
			y: 5,
      type: ["fractalNoise", "turbulence"],
      stitchTiles: ["noStitch", "stitch"],
			easeList: [
				"Power0.easeNone",
				"Power1.easeIn",
				"Power1.easeInOut",
				"Power1.easeOut",
				"Power2.easeIn",
				"Power2.easeInOut",
				"Power2.easeOut",
				"Power3.easeIn",
				"Power3.easeInOut",
				"Power3.easeOut",
				"Power4.easeIn",
				"Power4.easeInOut",
				"Power4.easeOut",
				"Back.easeIn",
				"Back.easeOut",
				"Back.easeInOut",
				"Elastic.easeIn",
				"Elastic.easeOut",
				"Elastic.easeInOut",
				"Bounce.easeIn",
				"Bounce.easeOut",
				"Bounce.easeInOut",
				"Circ.easeIn",
				"Circ.easeOut",
				"Circ.easeInOut",
				"Expo.easeIn",
				"Expo.easeOut",
				"Expo.easeInOut",
				"Sine.easeIn",
				"Sine.easeOut",
				"Sine.easeInOut",
			]
		};

		// image
		let folderImage = gui.addFolder("Image");
    folderImage.add(params, "size", 10, 100).onChange(()=>{
			this.image.width = params.size + "%";
			this.image.height = params.size + "%";
		});
    folderImage.add(params, "x", 0, 100).onChange(()=>{
			this.image.x = params.x + "%";
		});
    folderImage.add(params, "y", 0, 100).onChange(()=>{
			this.image.y = params.y + "%";
		});

		// filter
		let folderFilter = gui.addFolder("Filter");
		folderFilter.open();
    folderFilter.add(this.filter, "type", params.type);
		folderFilter.add(this.filter, "baseFrequencyX", 0, 1).step(0.01);
    folderFilter.add(this.filter, "baseFrequencyY", 0, 1).step(0.01);
		folderFilter.add(this.filter, "numOctaves", 1, 10).step(1);
	  folderFilter.add(this.filter, "seed", 0, 10).step(1);
    folderFilter.add(this.filter, "stitchTiles", params.stitchTiles);
		folderFilter.add(this.filter, "scale", 0, 300).step(1);

		// tween
		gui.add(this, "duration", 0.1, 3.0);
		gui.add(this, "ease", params.easeList);
	},

	methods: {
		onActive(){
			this.$refs.svg.onActive();
		},
		onPassive(){
			this.$refs.svg.onPassive();
		}
	}
}
</script>


/*==========================================================================
	style
==========================================================================*/
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.svg{
	width: 640px;
  height: 360px;
  cursor: pointer;
	background: #e9e9e9;
}
</style>
