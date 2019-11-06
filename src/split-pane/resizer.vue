<template>
	<div class="splitter-pane-resizer"></div>
</template>

<script>
export default {}
</script>

<style scoped>
.splitter-pane-resizer {
	box-sizing: border-box;
	background-color: #000;
	z-index: 1;
	background-clip: padding-box;

  width: 11px;
	height: 100%;
	margin-left: -5px;
	border-left: 5px solid rgba(255, 255, 255, 0);
	border-right: 5px solid rgba(255, 255, 255, 0);
	cursor: col-resize;
}
</style>
