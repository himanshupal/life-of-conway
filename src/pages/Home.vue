<template>
	<div class="min-h-screen min-w-max grid place-content-center text-center dark:bg-gray-900 dark:text-gray-100">
		<div class="flex flex-col gap-0.5">
			<div v-for="(row, ok) in grid" :key="`row-${ok}`" class="flex gap-0.5">
				<div
					v-for="(cell, ik) in row"
					:key="`cell-${ik}`"
					:class="[`w-1 h-1 transition ease-out`, row[ik] ? `bg-blue-400` : `bg-gray-700`]"
					@mouseover="grid = grid.map((row, a_ok) => row.map((_, a_ik) => a_ok === ok && a_ik === ik))"
					@mouseleave="grid = grid.map((row, a_ok) => row.map((_, a_ik) => !a_ok === ok && a_ik === ik))"
				/>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

const CELLS: number = 100

interface Data {
	grid: Array<Array<Boolean>>
}

export default defineComponent({
	name: 'Home',

	data(): Data {
		return {
			grid: new Array(CELLS).fill(new Array(CELLS).fill(false))
		}
	}
})
</script>
