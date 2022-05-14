<template>
	<view @click="$jump(setting.jumpPath)" class="mc-button" :style="{'height': $unit(setting.height)}">
		<u-button v-if="setting.hoterea == false" :type="setting.type" :text="setting.title">{{setting.title}}</u-button>
		 <!-- :style="{'border': store.getters.onBuilding ? '1px yellow dashed' : 'none'}" -->
		<view v-if="setting.hoterea == true" class="hot-area"></view>
	</view>
</template>

<script>
	export default {
		name: 'McButton',

		props: {
			setting: {
				type: Object,
				default: () => {}
			}
		}

	}
</script>

<style lang="scss" scoped>
	.mc-button {
		width: 100%;
		height: 100%;
		overflow: hidden;
		z-index: 0;
		.hot-area {
			width: 100%;
			height: 100%;
			display: block;
		}
	}
</style>
