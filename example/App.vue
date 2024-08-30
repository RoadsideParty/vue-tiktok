<script setup lang="ts">
import { ref } from "vue"
import { tiktok } from "../lib"
// import { tiktok } from "vue-tiktok"

const list = ref([1, 2, 3, 4, 5])
</script>

<template>
	<button style="position: fixed; right: 0">加载更多</button>
	<tiktok :list>
		<template #="{ item }">
			<div class="item">{{ item }}</div>
		</template>
	</tiktok>
</template>

<style lang="scss">
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
</style>
