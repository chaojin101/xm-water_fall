<script setup lang="ts">
import { ref, onMounted } from "vue";

const props = defineProps<{
	list: any[];
}>();

const waterList = ref<any[]>([]);
const heightList: number[] = [];

const init = () => {
	const width = 130;
	const x = document.body.clientWidth;
	const column = Math.floor(x / width);

	for (let i = 0; i < props.list.length; i++) {
		if (i < column) {
			// 摆放第一行
			props.list[i].left = i * width;
			props.list[i].top = 20;
			waterList.value.push(props.list[i]);
			heightList.push(props.list[i].height + 20);
		} else {
			let minHeightIndex = heightList.reduce(
				(preIndex, cur, curIndex) =>
					heightList[preIndex] <= cur ? preIndex : curIndex,
				0
			);
			props.list[i].top = heightList[minHeightIndex] + 20;
			props.list[i].left = width * minHeightIndex;
			waterList.value.push(props.list[i]);
			heightList[minHeightIndex] += props.list[i].height + 20;
		}
	}
};

onMounted(() => {
	init();
});
</script>

<template>
	<div class="wraps">
		<div
			v-for="item in waterList"
			class="items"
			:style="{
				height: item.height + 'px',
				background: item.background,
				left: item.left + 'px',
				top: item.top + 'px',
			}"
		></div>
	</div>
</template>

<style scoped lang="less">
.wraps {
	position: relative;
	.items {
		position: absolute;
		width: 120px;
	}
}
</style>
