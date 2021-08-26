<template>
	<div
		class="
			min-h-screen min-w-max
			grid
			place-content-center
			text-center
			bg-green-400
			dark:bg-gray-900 dark:text-gray-100
		"
	>
		<div class="flex flex-col">
			<div v-for="(row, ok) in grid" :key="`row-${ok}`" class="flex">
				<div
					v-for="(cell, ik) in row"
					:key="`cell-${ik}`"
					:class="[
						`w-2.5 h-2.5 border border-dotted border-green-600 dark:border-pink-500`,
						row[ik] ? `bg-blue-50 dark:bg-blue-400` : `dark:bg-gray-700`
					]"
					@mouseenter="userInput(ok, ik)"
				/>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

let cellsX: number = 75
let cellsY: number = 75

interface Data {
	grid: Array<Array<Boolean>>
}

export default defineComponent({
	name: 'GoLife',

	data(): Data {
		return {
			grid: new Array(cellsY).fill(new Array(cellsX).fill(false))
		}
	},

	methods: {
		userInput(ok: number, ik: number) {
			const t_row = this.grid[ok].map((cell, a_ik) => (a_ik === ik ? !cell : cell))
			this.grid = this.grid.map((row, a_ok) => (a_ok === ok ? t_row : row))
		}
	},

	beforeCreate() {
		// 10 will make it touch edges without overflow
		cellsX = Math.floor(innerWidth / 11.5)
		cellsY = Math.floor(innerHeight / 12.5)
	}
})
</script>
