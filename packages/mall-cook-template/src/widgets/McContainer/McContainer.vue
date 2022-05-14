<template>
	<view class="container-block" :style="[containerStyles()]">
		<!-- #ifdef H5 -->
		<VueDragResize 
			v-bind="getRectSetting(item)" 
			v-for="(item, index) in list" 
			:key="item.id" 
			:isActive="curWidgetId == item.id"
			@click.native.stop="setCurWidgetId(item.id)" 
			@dragging="curWidgetIndex = index;curWidgetId = item.id;onDragging = true;" 
			@resizing="curWidgetIndex = index;curWidgetId = item.id"
			@clicked="curWidgetIndex = index;curWidgetId = item.id" 
			@resizestop="resizeStop" 
			@dragstop="resizeStop"
		>
			<div v-show="onDragging && curWidgetId == item.id">
				<div class="line_transverse"></div>
				<div class="line_vertical"></div>
			</div>
			<widget-shape-simple :style="[itemStyles(item, true)]" class="drag-item" v-if="item" :widget="item">
				<view v-show="curWidgetId == item.id" class="item-opt">
					<u-icon @click="delWidget(index)" color="red" size="40" name="trash"></u-icon>
				</view>
				<render-widget-simple :item="item"></render-widget-simple>
			</widget-shape-simple>
		</VueDragResize>
		<!-- #endif -->
		<!-- #ifndef H5 -->
		<view class="container-item" :style="[itemStyles(item)]" v-for="item in list" :key="item.id">
			<widget-shape-simple v-if="item" :widget="item">
				<render-widget-simple :item="item"></render-widget-simple>
			</widget-shape-simple>
		</view>
		<!-- #endif -->
	  <view class="empty" v-if="list.length == 0">请给容器添加组件~</view>
	</view>
</template>

<script>
	import WidgetShapeSimple from "./widget-shape-simple";
	import RenderWidgetSimple from "./render-widget-simple";
	// #ifdef H5
	import VueDragResize from '@/utils/vue-drag-resize.js';
	// #endif

	export default {
		// #ifdef H5
  		inject: ["page"],
		// #endif
		components: {
			WidgetShapeSimple,
			RenderWidgetSimple,
			// #ifdef H5
			VueDragResize
			// #endif
		},
		name: 'McContainer',
		data() {
			return {
				curWidgetIndex: '',
				curWidgetId: '',
				onDragging: false
			}
		},
		methods: {
			getRectSetting(rect) {
				return {
					w: rect.setting.width || 80,
					h: rect.setting.height || 80,
					x: rect.setting.left || 0,
					y: rect.setting.top || 0,
					minw: 10,
					minh: 10
				};
			},
			containerStyles() {
				if(!this.attrs) return {};
				let result = {};
				result['background'] = this.attrs.conBackground || '';
				result['height'] = this.$unit(this.attrs.conHeight) || '200rpx';
				return result;
			},
			itemStyles(item, onDrag = false) {
				if(!item.setting) return {};
				let result = {};
				result['width'] = this.$unit(item.setting.width) || '80px';
				result['height'] = this.$unit(item.setting.height) || '80px';
				if(!onDrag) {
					result['top'] = this.$unit(item.setting.top) || '0rpx';
					result['left'] = this.$unit(item.setting.left) || '0rpx';
				}
				return result;
			},
			setCurWidgetId(id) {
				this.page.setCurWidgetId(id, this.widgetId);
			},
			delWidget(index) {
				this.list.splice(index, 1);
				this.page.setList();
			},
			debounce(fn, wait) {
				let timer;
				return function () {
					const context = this;
					const args = [...arguments];
					if (timer) clearTimeout(timer);
					timer = setTimeout(() => {
						fn.apply(context, args);
					}, wait);
				};
			},
			resizeStop(newRect) {
				if(this.curWidgetIndex !== '') {
					this.list[this.curWidgetIndex].setting.width = newRect.width;
					this.list[this.curWidgetIndex].setting.height = newRect.height;
					this.list[this.curWidgetIndex].setting.left = newRect.left;
					this.list[this.curWidgetIndex].setting.top = newRect.top;
					this.curWidgetIndex = '';
					this.page.setList();
				}
				this.onDragging = false;
			}
		},

		props: {
			attrs: {
				type: Object,
				default: () => {}
			},
			list: {
				type: Array,
				default: () => []
			},
			widgetId: {
				type: String,
				default: ''
			}
		}
	}
</script>
<style lang="scss" scoped>
	.container-block {
		position: relative;
		overflow: hidden;
		.empty {
			width: 100%;
			text-align: center;
			color: #999;
			margin-top: 20px;
		}
		.container-item {
			position: absolute;
			overflow: hidden;
		}
		.line_vertical, .line_transverse {
			display: block;
			position: absolute;
			z-index: 999;
			border-color: blue !important;
		}
		.line_vertical {
			height: 1500px;
			top: -500px;
			left: 0;
			border-left: 1px #000 dashed;
		}
		.line_transverse {
			width: 1500px;
			top: 0;
			left: -500px;
			border-bottom: 1px #000 dashed;
		}
		.drag-item {
			position: relative;
			overflow: hidden;
			.item-opt {
				position:absolute;
				right: 0px;
				top: 0px;
				z-index: 99999;
				cursor: pointer;
			}
			&:hover {
				border: 1px blue dotted;
			}
		}
	}
</style>