<template>
  <div @mousedown="mouseDown" @mouseup="mouseUp" id="Page">
	<Box v-for="(box) in boxes" :left="box.left" :top="box.top" :width="box.width" :height="box.height" :text="box.text" :center="box.center" :textColor="box.textColor" :backgroundColor="box.backgroundColor" :fontSize="box.fontSize" :borderSize="box.borderSize" :borderRadius="box.borderRadius" :borderColor="box.borderColor"></Box>
  </div>
  <div id="Inspector">
	<div id="WidthLabel"><b>WIDTH</b></div>
	<input id="Width" class="slide" type="range" v-model="width" min="10" max="1000">
	<div id="HeightLabel"><b>HEIGHT</b></div>
	<input id="Height" class="slide" type="range" v-model="height" min="10" max="600">
	<div id="TextLabel"><b>TEXT</b></div>
	<input id="Text" v-model="text">
	<div id="CenterLabel"><b>CENTERING</b></div>
	<button @click="centerLeft" id="CenterLeft"></button>
	<button @click="centerMiddle" id="CenterMiddle" class="centerSelected"></button>
	<button @click="centerRight" id="CenterRight"></button>
	<div id="ColorLabel"><b>COLORS</b></div>
	<input id="TextColor" v-model="textColor">
	<div id="TextColorLabel"><b>TEXT</b></div>
	<input id="BackgroundColor" v-model="backgroundColor">
	<div id="BackgroundColorLabel"><b>BACKGROUND</b></div>
	<div id="FontSizeLabel"><b>FONT SIZE</b></div>
	<input id="FontSize" class="slide" type="range" v-model="fontSize" min="10" max="200">
	<div id="BorderLabel"><b>BORDER</b></div>
	<input id="BorderSize" v-model="borderSize">
	<div id="BorderSizeLabel"><b>SIZE</b></div>
	<input id="BorderRadius" v-model="borderRadius">
	<div id="BorderRadiusLabel"><b>RADIUS</b></div>
	<input id="BorderColor" v-model="borderColor">
	<div id="BorderColorLabel"><b>COLOR</b></div>
  </div>
</template>

<script>
import Box from "./Box.vue"
export default {
	data(){
		return{
			boxes:[],
			mousedown:"no",
			mouseX:0,
			mouseY:0,
			boxDisplaceX:0,
			boxDisplaceY:0,
			boxSelected:-1,
			boxInspector:0,
			width:100,
			height:100,
			text:"",
			textColor:"",
			backgroundColor:"",
			fontSize:"",
			borderSize:"",
			borderRadius:"",
			borderColor:"",
		}
	},
	components:{
		Box
	},
	watch:{
		width(newWidth){
			this.boxes[this.boxInspector].width=newWidth+"px";
		},
		height(newHeight){
			this.boxes[this.boxInspector].height=newHeight+"px";
		},
		text(newText){
			this.boxes[this.boxInspector].text=newText;
		},
		textColor(newTextColor){
			this.boxes[this.boxInspector].textColor=newTextColor;
		},
		backgroundColor(newBackgroundColor){
			this.boxes[this.boxInspector].backgroundColor=newBackgroundColor;
		},
		fontSize(newFontSize){
			this.boxes[this.boxInspector].fontSize=newFontSize+"px";
		},
		borderSize(newBorderSize){
			this.boxes[this.boxInspector].borderSize=newBorderSize+"px";
		},
		borderRadius(newBorderRadius){
			this.boxes[this.boxInspector].borderRadius=newBorderRadius+"px";
		},
		borderColor(newBorderColor){
			this.boxes[this.boxInspector].borderColor=newBorderColor;
		},
	},
	methods:{
		mouseDown:function(){
			this.mousedown="yes";
			for(var i=0;i<this.boxes.length;i++){
				var x=parseInt(this.boxes[i].left.substring(0,this.boxes[i].left.length-2))+96;
				var y=parseInt(this.boxes[i].top.substring(0,this.boxes[i].top.length-2))+100;
				var width=parseInt(this.boxes[i].width.substring(0,this.boxes[i].width.length-2));
				var height=parseInt(this.boxes[i].height.substring(0,this.boxes[i].height.length-2));
				if(this.mouseX>x && this.mouseX<x+width && this.mouseY>y && this.mouseY<y+height){
					this.boxSelected=i;
					this.boxInspector=i;
					this.boxDisplaceX=this.mouseX-x;
					this.boxDisplaceY=this.mouseY-y;
					this.width=width;
					this.height=height;
					this.text=this.boxes[i].text;
					if(this.boxes[i].center=="left") this.centerLeft();
					if(this.boxes[i].center=="center") this.centerMiddle();
					if(this.boxes[i].center=="right") this.centerRight();
					this.textColor=this.boxes[i].textColor;
					this.backgroundColor=this.boxes[i].backgroundColor;
					this.fontSize=this.boxes[i].fontSize.substring(0,this.boxes[i].fontSize.length-2);
					this.borderSize=this.boxes[i].borderSize.substring(0,this.boxes[i].borderSize.length-2);
					this.borderRadius=this.boxes[i].borderRadius.substring(0,this.boxes[i].borderRadius.length-2);
					this.borderColor=this.boxes[i].borderColor;
					break;
				}
			}
		},
		mouseUp:function(){
			this.mousedown="no";
			this.boxSelected=-1;
		},
		update(){
			if(this.mousedown=="yes"){
				if(this.boxSelected!=-1){
					this.boxes[this.boxSelected].left=(this.mouseX-96-this.boxDisplaceX)+"px";
					this.boxes[this.boxSelected].top=(this.mouseY-100-this.boxDisplaceY)+"px";
				}
			}
		},
		centerLeft(){
			this.boxes[this.boxInspector].center="left"
			document.getElementById("CenterLeft").classList.add("centerSelected");
			document.getElementById("CenterMiddle").classList.remove("centerSelected");
			document.getElementById("CenterRight").classList.remove("centerSelected");
		},
		centerMiddle(){
			this.boxes[this.boxInspector].center="center"
			document.getElementById("CenterLeft").classList.remove("centerSelected");
			document.getElementById("CenterMiddle").classList.add("centerSelected");
			document.getElementById("CenterRight").classList.remove("centerSelected");
		},
		centerRight(){
			this.boxes[this.boxInspector].center="right"
			document.getElementById("CenterLeft").classList.remove("centerSelected");
			document.getElementById("CenterMiddle").classList.remove("centerSelected");
			document.getElementById("CenterRight").classList.add("centerSelected");
		},
	},
	mounted(){
		this.$nextTick(function () {
            window.setInterval(() => {
                this.update();
            },10);
		});
		document.addEventListener("mousemove", (event) => {
			this.mouseX = event.clientX;
			this.mouseY = event.clientY;
		});


	},
	created() {
		window.addEventListener('keydown', (e) => {
		  if (e.key == 'Enter') {
			this.boxes.push({left:"200px",top:"200px",width:"200px",height:"100px",text:"Hello!",center:"center",textColor:"#000",backgroundColor:"#0000",fontSize:"20px",borderSize:"2px",borderRadius:"10px",borderColor:"#000"});
		  }
		});
	},
	
	
	
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');
body{
	overflow:hidden;
	font-family: 'Open Sans', sans-serif;
}
#Page{
	border:2px solid rgba(0,0,0,0.4);
	border-radius:20px;
	width:75%;
	height:75%;
	position:absolute;
	left:5%;
	top:10%;
}
#Inspector{
	border-left:2px solid rgba(0,0,0,0.2);
	width:15%;
	height:100%;
	position:absolute;
	left:85%;
	top:0%;
	
}
#WidthLabel{
	position:absolute;
	left:15%;
	top:5%;
	font-size:25px;
}
#Width{
	position:absolute;
	left:15%;
	top:10%;
}
#HeightLabel{
	position:absolute;
	left:15%;
	top:16%;
	font-size:25px;
}
#Height{
	position:absolute;
	left:15%;
	top:21%;
}
#TextLabel{
	position:absolute;
	left:15%;
	top:27%;
	font-size:25px;
}
#Text{
	position:absolute;
	left:15%;
	top:32%;
	width:67%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:20px;
	text-align:center;
	color:rgba(0,0,0,0.7);
}
#Text:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#CenterLabel{
	position:absolute;
	left:15%;
	top:40%;
	font-size:25px;
}
#CenterLeft{
	height: 35px;
	 width: 35px;
	 border:2px solid #04AA6D;
	 border-radius: 50%;
	 position:absolute;
	 top:45%;
	 left:15%;
	 transition:0.1s all;
}
#CenterLeft:hover{
	box-shadow: 0 0 0 7px rgba(4, 170, 109, .2)
}
#CenterMiddle{
	height: 35px;
	 width: 35px;
	 border:2px solid #04AA6D;
	 border-radius: 50%;
	 position:absolute;
	 top:45%;
	 left:43%;
	transition:0.1s all;
}
#CenterMiddle:hover{
	box-shadow: 0 0 0 7px rgba(4, 170, 109, .2)
}
#CenterRight{
	height: 35px;
	 width: 35px;
	 border:2px solid #04AA6D;
	 border-radius: 50%;
	 position:absolute;
	 top:45%;
	 left:72%;
	 transition:0.1s all;
}
#CenterRight:hover{
	box-shadow: 0 0 0 7px rgba(4, 170, 109, .2)
}
.centerSelected{
	background-color: #04AA6D;
	transform:scale(1.2)
}
#ColorLabel{
	position:absolute;
	left:15%;
	top:53%;
	font-size:25px;
}
#TextColor{
	position:absolute;
	left:15%;
	top:58%;
	width:30%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:17px;
	text-align:center;
	color:rgba(0,0,0,0.7)
}
#TextColor:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#TextColorLabel{
	position:absolute;
	left:25%;
	top:64%;
	font-size:15px;
	color:rgba(0,0,0,0.7);
}
#BackgroundColor{
	position:absolute;
	left:55%;
	top:58%;
	width:30%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:20px;
	text-align:center;
	color:rgba(0,0,0,0.7);
}
#BackgroundColor:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#BackgroundColorLabel{
	position:absolute;
	left:55%;
	top:64%;
	font-size:15px;
	color:rgba(0,0,0,0.7);
}
#FontSizeLabel{
	position:absolute;
	left:15%;
	top:69%;
	font-size:25px;
}
#FontSize{
	position:absolute;
	left:15%;
	top:75%;
}
#BorderLabel{
	position:absolute;
	left:15%;
	top:79%;
	font-size:25px;
}
#BorderSize{
	position:absolute;
	left:15%;
	top:84%;
	width:15%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:20px;
	text-align:center;
}
#BorderSize:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#BorderSizeLabel{
	position:absolute;
	left:20%;
	top:89.8%;
	font-size:15px;
	color:rgba(0,0,0,0.7);
}
#BorderRadius{
	position:absolute;
	left:37%;
	top:84%;
	width:15%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:20px;
	text-align:center;
}
#BorderRadius:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#BorderRadiusLabel{
	position:absolute;
	left:37.5%;
	top:89.8%;
	font-size:15px;
	color:rgba(0,0,0,0.7);
}
#BorderColor{
	position:absolute;
	left:60%;
	top:84%;
	width:30%;
	height:50px;
	border:2px solid rgba(0,0,0,0.2);
	border-radius:10px;
	transition:0.2s all;
	font-family: 'Open Sans', sans-serif;
	padding-left:5px;
	font-size:17px;
	text-align:center;
	color:rgba(0,0,0.7);
}
#BorderColor:focus{
	border:2px solid #04AA6D;
	outline:none;
}
#BorderColorLabel{
	position:absolute;
	left:67.5%;
	top:89.8%;
	font-size:15px;
	color:rgba(0,0,0,0.7);
}
.slide{
	-webkit-appearance: none;
	 appearance: none; 
	 width: 70%;
	 cursor: pointer;
	 outline: none;
	 border-radius: 15px;
	 height: 8px;
	 background: rgba(0,0,0,0.2);
	transition:0.2s all;
}
.slide::-webkit-slider-thumb{
	-webkit-appearance: none;
	 appearance: none; 
	 height: 25px;
	 width: 25px;
	 background-color: #04AA6D;
	 border-radius: 50%;
	 border: none;
	 transition:0.2s all;
}
.slide::-webkit-slider-thumb:hover{
	box-shadow: 0 0 0 13px rgba(4, 170, 109, .2)
}
</style>
