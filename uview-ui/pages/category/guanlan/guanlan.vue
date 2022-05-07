<template>
	<view>
        <u-navbar :is-back="false" title="观澜" :background="background" :title-color="titleColor"></u-navbar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
