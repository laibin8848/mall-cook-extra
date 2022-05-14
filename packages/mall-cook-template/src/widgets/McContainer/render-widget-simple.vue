<template>
  <view class="render">
    <McText
      v-if="item.component == 'McText'"
      :id="'widget' + item.id"
      :key="item.id"
      :setting="item.setting"
    ></McText>
    <McButton
      v-if="item.component == 'McButton'"
      :id="'widget' + item.id"
      :key="item.id"
      :setting="item.setting"
    ></McButton>
    <McSimpleImg
      v-if="item.component == 'McSimpleImg'"
      :id="'widget' + item.id"
      :key="item.id"
      :setting="item.setting"
    ></McSimpleImg>

    <McCountdown
      v-if="item.component == 'McCountdown'"
      :id="'widget' + item.id"
      :key="item.id"
      :styles="item.styles"
      :value="item.value"
    ></McCountdown>
  </view>
</template>

<script>

export default {
  props: {
    item: {
      type: Object,
      default: () => {},
    },
  },
};
</script>