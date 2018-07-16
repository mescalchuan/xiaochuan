<template>
    <div :class = "['x-sheet-container', sheetHasShown ? 'x-sheet-active' : '']" >
        <div class = "x-sheet-mask" data-tap @click="closeSheet" ></div>
        <div class = "x-sheet-view x-sheet-view-bottom" >
            <slot></slot>
        </div>
    </div>
</template>

<script type="text/javascript">
export default {
    name: "ActionSheet",
    props: {
		sheetOpen: {
			type: Function,
			default: () => {}
		},
		sheetClose: {
			type: Function,
			default: () => {}
		}
	},
	data() {
		return {
			openValue: 0,
			sheetHasShown: false,
			scrollTop: 0
		}
	},
	methods: {
		openSheet() {
			this.sheetHasShown = true;
			setTimeout(() => {
				this.sheetOpen && this.sheetOpen();
			}, 300)
		},
		closeSheet() {
			this.sheetHasShown = false;
			setTimeout(() => {
				this.sheetClose && this.sheetClose();
			}, 300)
		}
	},
}
</script>

<style scoped>
.x-sheet-container {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    pointer-events: none;
    z-index: 99;
}

.x-sheet-container .x-sheet-mask {
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    transition: opacity 0.3s;
    position: absolute;
    opacity: 0;
    background-color: rgba(0, 0, 0, 0.5);
}

.x-sheet-view {
    width: 100vw;
    background-color: white;
    /* height: 150px; */
    overflow: auto;
    position: absolute;
    transition: transform 0.3s;
    -webkit-transition: -webkit-transform 0.3s;
}

.x-sheet-view-bottom {
	bottom: 0;
    transform: translateY(150px);
    -webkit-transform: translateY(150px);
}

.x-sheet-active {
    pointer-events: auto;
}

.x-sheet-active .x-sheet-view-bottom {
   transform: translateY(0);
   -webkit-transform: translateY(0);
}

.x-sheet-active .x-sheet-mask {
    opacity: 1;
}
</style>
