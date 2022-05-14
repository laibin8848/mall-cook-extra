<template>
	<view class="mc-text" :style="{'color': setting.color, 'font-size': $unit(setting.fontSize)}">
		{{setting.label}}
	</view>
</template>

<script>
	export default {
		name: 'McText',

		props: {
			setting: {
				type: Object,
				default: () => {}
			}
		},

	}
</script>

<style lang="scss" scoped>
	.mc-text {
		overflow: hidden;
	}
</style>
