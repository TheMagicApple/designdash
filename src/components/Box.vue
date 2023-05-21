<template>
	<div :style="css">{{text}}</div>
</template>
<script>
export default{
	props:["left","top","width","height","text","center","textColor","backgroundColor","fontSize","borderSize","borderRadius","borderColor"],
	computed:{
		css(){
			return{
				"--left":this.left,
				"--top":this.top,
				"--width":this.width,
				"--height":this.height,
				"--center":this.center,
				"--textColor":this.textColor,
				"--backgroundColor":this.backgroundColor,
				"--fontSize":this.fontSize,
				"--borderSize":this.borderSize,
				"--borderRadius":this.borderRadius,
				"--borderColor":this.borderColor,
			}
		}
	},
	mounted(){
		
	}
}
</script>
<style scoped>
div{
	position:absolute;
	left:var(--left);
	top:var(--top);
	width:var(--width);
	height:var(--height);
	display:flex;
	align-items:center;
	justify-content:var(--center);
	color:var(--textColor);
	background-color:var(--backgroundColor);
	font-size:var(--fontSize);
	border:var(--borderSize) solid var(--borderColor);
	border-radius:var(--borderRadius);
	cursor:pointer;
	user-select: none;
}
</style>