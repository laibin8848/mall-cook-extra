<!--
 * @Description: 物料列表 
 * @Autor: WangYuan
 * @Date: 2021-05-19 10:41:34
 * @LastEditors: WangYuan
 * @LastEditTime: 2022-02-12 14:41:25
-->

<template>
  <div class="wrap">
    <div class="w-100 h-100 p16 bg-white">
      <ul class="flex flex-wrap">
        <li
          v-for="(item, index) in widgets"
          :key="index"
          class="item"
          :data-component="item.component"
          draggable
          @dragstart="handleDragStart"
          @dragend="dragEnd"
        >
          <i class="icon" :class="item.icon"></i>
          <span>{{ item.name }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  inject: ["control"],
  computed: {
    //如果当前是自由容器，组件面板仅展示基本组件
    widgets() {
      const widgets = {};
      if(this.control.curWidget && this.control.curWidget.component == this.globalContainerName) {
        for(const key in this.$initializing) {
          if(this.$simpleComponents.includes(this.$initializing[key].component)) {
            widgets[key] = this.$initializing[key];
          }
        }
      } else {
        for(const key in this.$initializing) {
          if(!this.$simpleComponents.includes(this.$initializing[key].component)) {
            widgets[key] = this.$initializing[key];
          }
        }
      }
      return widgets;
    }
  },

  methods: {
    // 拖拽开始
    handleDragStart(e) {
      this.control.dragWidget = this.$getNewComponent(
        e.target.dataset.component
      );
      this.control.dragstatus = true;
    },

    // 拖拽结束
    dragEnd(e) {
      // 如果当前是给容器添加组件
      if(this.control.curWidget && this.control.curWidget.component == this.globalContainerName) {
        return;
      }
      this.control.h5Iframe.contentWindow.postMessage(
        {
          even: "drop",
          params: this.control.dragWidget,
        },
        "*"
      );

      this.control.dragstatus = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.wrap {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 176px; /*no*/
  overflow: hidden;
}

.item {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 72px; /*no*/
  margin-bottom: 8px; /*no*/
  padding: 10px 0; /*no*/
  font-size: 12px; /*no*/
  color: #666;

  i {
    font-size: 29px; /*no*/
    margin-top: 5px; /*no*/
    margin-bottom: 10px; /*no*/
  }

  &:hover {
    color: #fff;
    background: $color-theme;
  }
}
</style>
