<script setup lang="ts">
import { onMounted, ref } from "vue"

const props = withDefaults(defineProps<Props>(), {
	threshold: 100,
})
const { list, threshold } = props

// 开始的触点
const touchY = ref(0)
// 本次移动的距离
let currentMoveY = 0
// 当前已经移动的距离(累计)
let currentTranslateY = 0
// 最终移动距离
const translateY = ref(0)
// 当前移动到了哪一个
let currentIndex = 0

function touchstart(e: TouchEvent) {
	const { clientY } = e.touches[0]
	touchY.value = clientY
}

function touchmove(e: TouchEvent) {
	const { clientY } = e.touches[0]
	currentMoveY = clientY - touchY.value
	translateY.value = currentTranslateY + currentMoveY
}

function touchend(e: TouchEvent) {
	if (currentMoveY >= threshold && Math.abs(currentIndex) > 0) {
		currentIndex += 1
	}
	if (currentMoveY <= -threshold && Math.abs(currentIndex) < list.length - 1) {
		currentIndex -= 1
	}
	translateY.value = currentIndex * tiktokHeight.value
	currentTranslateY = translateY.value
}

const tiktokMainRef = ref<HTMLDivElement>()
const tiktokHeight = ref(0)
function init() {
	const { height } = (
		tiktokMainRef.value as HTMLDivElement
	).getBoundingClientRect()
	tiktokHeight.value = height
}

onMounted(() => {
	init()
})
</script>

<template>
	<div class="tiktok-main" ref="tiktokMainRef">
		<div
			class="tiktok-wrap"
			:style="{ transform: `translateY(${translateY + 'px'})` }"
		>
			<div
				class="tiktok-item"
				v-for="(item, index) in list"
				:key="index"
				@touchstart="touchstart"
				@touchmove="touchmove"
				@touchend="touchend"
			>
				<slot :item :index />
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
.tiktok-main {
	height: 100vh;
	overflow: hidden;
	.tiktok-wrap {
		height: 100%;
		transition: all 0.3s linear;
		.tiktok-item {
			height: 100%;
		}
	}
}
</style>
