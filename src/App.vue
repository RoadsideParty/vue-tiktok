<script setup lang="ts">
import { tiktok } from "./lib"
</script>

<template>
	<tiktok :list="[1, 2, 3]">
		<template #="{ item }">
			<div class="item">{{ item }}</div>
		</template>
	</tiktok>
</template>

<style lang="scss" scoped></style>
