/*==========================================================================
	template
==========================================================================*/
<template>
  <svg
		@mouseenter="$emit('mouseenter')"
		@mouseleave="$emit('mouseleave')"
	>
    <filter id="distortion">
      <feTurbulence
        :type="filter.type"
        :baseFrequency="baseFrequency"
        :numOctaves="filter.numOctaves"
        :seed="filter.seed"
        :stitchTiles="filter.stitchTiles"
        result="noise"
      ></feTurbulence>
      <feDisplacementMap
        in="SourceGraphic"
        in2="noise"
	      :scale="scale"
        xChannelSelector="R"
        yChannelSelector="G"
        filterUnits="userSpaceOnUse"
      ></feDisplacementMap>
    </filter>
    <g filter="url(#distortion)">
			<image
				:xlink:href="image.passive"
				:width="image.width"
				:height="image.height"
				:x="image.x"
				:y="image.y"
				:opacity="opacity.passive"
			/>
			<image
				:xlink:href="image.active"
				:width="image.width"
				:height="image.height"
				:x="image.x"
				:y="image.y"
				:opacity="opacity.active"
			/>
      <!-- <rect class="debug" :x="x" :y="y" :width="width" :height="height" /> -->
    </g>
  </svg>
</template>


/*==========================================================================
	script
==========================================================================*/
<script>
import { TweenLite } from "gsap/TweenMax";


export default {
  name: "SvgDistotion",

  props: {
    image: {
      type: Object,
      default: () => {
        return {
					passive: "../../assets/img/img01.jpg",
					active: "../../assets/img/img02.jpg",
					width: "90%",
					height: "90%",
					x: "5%",
					y: "5%"
				};
      }
    },
    filter: {
      type: Object,
      default: () => {
				return {
					type: "fractalNoise",
					baseFrequencyX: 0.01,
					baseFrequencyY: 0.003,
					numOctaves: 2,
					seed: 2,
					stitchTiles: "noStitch",
					scale: 80,
					maxScale: 100
				}
			}
		},
		duration: {
			type: Number,
      default: 0.8
		},
		ease: {
			type: [String, Object],
      default: "Power2.easeOut"
		}
  },

  /**
   * @data
   */
  data() {
    return {
			// @private
			progress: 0
    };
  },

  /**
   * @computed
   */
  computed: {
    baseFrequency() {
      return this.filter.baseFrequencyX + " " + this.filter.baseFrequencyY;
    },

		scale(){
			let p = (0.5 - Math.abs(this.progress - 0.5)) * 2;
			return this.filter.scale * p;
		},

		opacity(){
			return {
				passive: 1 - this.progress,
				active: this.progress
			}
		}
  },

  /**
   * @methods
   */
	methods: {
		onPassive(){
      TweenLite.to(this, this.duration, {
        progress: 0,
        ease: this.ease
      });
		},

		onActive(){
      TweenLite.to(this, this.duration, {
        progress: 1,
        ease: this.ease
      });
		}
	}
};
</script>


/*==========================================================================
	style
==========================================================================*/
<style scoped>
</style>
