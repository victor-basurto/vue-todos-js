<script setup>

</script>
<template>
	<button>
		<slot></slot>
	</button>
</template>
<style lang="scss" scoped>
button {
	padding: 8px 16px;
	border: none;
	background-color: var(--color-secondary);
	cursor: pointer;
	transition: all 200ms ease-out;
	&:hover {
		background-color: var(--color-primary);
		color: #fff;
	}
}
</style>
