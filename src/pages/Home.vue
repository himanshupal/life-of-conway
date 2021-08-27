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
						`w-2.5 h-2.5 border border-dashed border-green-600 dark:border-pink-500`,
						grid[ok][ik] ? `bg-blue-50 dark:bg-blue-400` : `dark:bg-gray-700`,
						{ 'cursor-pointer': inputEnabled }
					]"
					@mousedown="userInput(ok, ik)"
				/>
			</div>
		</div>
	</div>

	<div class="fixed bottom-3 right-3 flex flex-col gap-1">
		<button class="px-4 py-1 text-xs bg-gray-600 text-white" @click="inputEnabled = !inputEnabled">
			{{ inputEnabled ? `Disable` : `Enable` }} Interaction
		</button>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

let cellsX: number = 75
let cellsY: number = 75

interface Data {
	inputEnabled: boolean
	grid: Array<Array<Boolean>>
}

const CellState = {
	DEAD: false,
	ALIVE: true
}

export default defineComponent({
	name: 'GoLife',

	data(): Data {
		let grid: Array<Array<boolean>> = new Array(cellsY).fill(new Array(cellsX).fill(CellState.DEAD))

		return {
			inputEnabled: true,
			grid
		}
	},

	methods: {
		detectNeighbour(outerKey: number, innerKey: number, row: Array<boolean>) {
			const leftX = innerKey - 1 < 0 ? null : innerKey - 1
			const rightX = innerKey + 1 >= row.length ? null : innerKey + 1
			const centerX = innerKey
			const centerY = outerKey
			const topY = outerKey - 1 < 0 ? null : outerKey - 1
			const bottomY = outerKey + 1 >= this.grid.length ? null : outerKey + 1

			return {
				topLeft: leftX !== null && topY !== null ? this.grid[topY][leftX] : null,
				top: centerY !== null && topY !== null ? this.grid[topY][centerX] : null,
				topRight: rightX !== null && topY !== null ? this.grid[topY][rightX] : null,

				left: leftX !== null && centerY !== null ? this.grid[centerY][leftX] : null,
				center: centerY !== null && centerX !== null ? this.grid[centerY][centerX] : null,
				right: rightX !== null && centerY !== null ? this.grid[centerY][rightX] : null,

				bottomLeft: leftX !== null && bottomY !== null ? this.grid[bottomY][leftX] : null,
				bottom: centerX !== null && bottomY !== null ? this.grid[bottomY][centerX] : null,
				bottomRight: rightX !== null && bottomY !== null ? this.grid[bottomY][rightX] : null
			}
		},

		userInput(ok: number, ik: number) {
			if (this.inputEnabled) {
				this.grid = this.grid.map((row, a_ok) => {
					return a_ok === ok
						? row.map((cell, a_ik) => {
								console.table(
									this.detectNeighbour(
										ok,
										ik,
										row.map((cell) => cell.valueOf())
									)
								)
								return a_ik === ik ? !cell : cell
						  })
						: row
				})
			}
		}
	},

	beforeCreate() {
		// 10 will make it touch edges without overflow
		cellsX = Math.floor(innerWidth / 11.5)
		cellsY = Math.floor(innerHeight / 12.5)
	}
})
</script>
