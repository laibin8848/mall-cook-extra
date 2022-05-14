<template>
	<view class="mc-image">
		<image
			style="width: 100%;"
			mode="widthFix"
			:src="setting.url || defaultImage"
		/>
	</view>
</template>

<script>
	export default {
		name: 'McSimpleImg',
		data() {
			return {
			defaultImage:
				"https://img01.yzcdn.cn/public_files/2019/03/05/2b60ed750a93a1bd6e17fc354c86fa78.png!large.webp",
			};
		},

		props: {
			setting: {
				type: Object,
				default: () => {}
			}
		},

	}
</script>

<style lang="scss" scoped>
.mc-image {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
