<template>
    <div class="bottom-bar">
        <div class="bom-bar-bg">
        <div class="left-part">
                <div class="progress-bar">
                    <div class="load-prog"></div>
                    <div class="prog play-prog" v-bind:style="{width:loadProg+'%'}">
                        <span class="pos"></span>
                    </div>
                </div>
                <div class="song-msg-bar">
                    <div class="music-name ell">{{songName + '-' + singer}}</div>
                    <div class="music-time">
                        <span class="ct">{{currentTime | timeFormat}}</span>/<span>{{ duration | timeFormat}}</span>
                    </div>
                </div>
        </div>
        <div class="right-part">
            <div class="play-btn" v-on:touchend="clickPlayBtn">
                    <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" viewBox="0 0 420 420" style="enable-background:new 0 0 420 420;" xml:space="preserve" width="100%" height="100%">
                        <g id="play" v-bind:style="{display:isPlay ? '' : 'none'}">
                            <path d="M210,21c104.216,0,189,84.784,189,189s-84.784,189-189,189S21,314.216,21,210S105.784,21,210,21 M210,0   C94.031,0,0,94.024,0,210s94.031,210,210,210s210-94.024,210-210S325.969,0,210,0L210,0z" fill="#FFFFFF"/>
                            <path d="M293.909,187.215l-111.818-73.591C162.792,100.926,147,109.445,147,132.545V287.42c0,23.1,15.813,31.647,35.147,18.998   L293.86,233.31C313.187,220.647,313.208,199.913,293.909,187.215z M279.006,217.868l-99.295,64.981   c-6.44,4.221-11.711,1.372-11.711-6.328V143.437c0-7.7,5.264-10.535,11.697-6.3l99.33,65.366   C285.46,206.731,285.453,213.647,279.006,217.868z" fill="#FFFFFF"/>
                        </g>

                        <g id="pause" style="display:none" v-bind:style="{display:!isPlay ? '' : 'none'}">
                            <path d="M210,21c104.216,0,189,84.784,189,189s-84.784,189-189,189S21,314.216,21,210S105.784,21,210,21 M210,0   C94.031,0,0,94.024,0,210s94.031,210,210,210s210-94.024,210-210S325.969,0,210,0L210,0z" fill="#FFFFFF"/>
                            <g>
                                <path d="M259,108.941c-19.25,0-35,15.75-35,35v132.125c0,19.25,15.75,35,35,35s35-15.75,35-35V143.941    C294,124.691,278.25,108.941,259,108.941z M273,276.066c0,7.7-6.3,14-14,14s-14-6.3-14-14V143.941c0-7.7,6.3-14,14-14    s14,6.3,14,14V276.066z" fill="#FFFFFF"/>
                                <path d="M161,108.941c-19.25,0-35,15.75-35,35v132.125c0,19.25,15.75,35,35,35s35-15.75,35-35V143.941    C196,124.691,180.25,108.941,161,108.941z M175,276.066c0,7.7-6.3,14-14,14s-14-6.3-14-14V143.941c0-7.7,6.3-14,14-14    s14,6.3,14,14V276.066z" fill="#FFFFFF"/>
                            </g>
                        </g>
                    </svg>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'control',
    data () {
        return {
        
        }
	},
	computed:{
		loadProg: function() {
			return this.currentTime/this.duration*100
		}
	},
	props: ['isPlay','songName','singer','currentTime','duration'],
	methods: {
		clickPlayBtn:function(){
			this.$emit('clickPlayBtn');
		}
	},
    filters:{
		timeFormat:function(time,type){
			var surplusMinite,
                surplusSecond,
                cTime;
            //将剩余秒数转化为分钟
            surplusMinite = Math.floor((time / 60) % 60);
            //将剩余秒数转化为秒钟
            surplusSecond = Math.floor(time % 60);
            if(surplusMinite < 10){
                surplusMinite = '0' + surplusMinite;
            }
            if(surplusSecond < 10){
                surplusSecond = '0' + surplusSecond;
            }
            cTime = surplusMinite + ':' + surplusSecond;
            return cTime;
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus">
$norm=750;
transformRem(v)
    (v/$norm*10)rem

$pos='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAYCAYAAAD+vg1LAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAMXSURBVEhL3ZTdS9NhFMftTS9CZE3BF6IXEIKoi7oLxIheILrYRaDuKoi6VNhVIvpf1FWxIBSDEG+C6kJZIIIgilCQLQqdL3vfz23OuX57Op/H5/dza2ldd+A7Dud8v9/n7DnPVvffxBGDY4LjgnpBgwE5NXoO768B6agAYcPS0tLVbDb7tFQqLdu2XQSSf6FGD47hojnwAMf0hN/v91iW9bxcLtvqgKAnBwQDgUAzGqOtMXdM630+n7dQKIQQ23ZZ7ezsqny+oLLZbQ1yavQI4X5Eg9Z4VJnrSQWNyWTy5Z6gqLa28ocCDiGaIFrjgZcOTuCeTk5MTHTLPdr5/I6yrNw/AS4atHgYLz01Hyzh1NraWrBY3FWZTNZFOp5S4cGAWrh8Ti1cOqtzapUcNGjxMF7amGfDSR25XO47xFRqy8Xyk4AKdTSp962NGuRfRwarOGjQ4mG88NSjNwnOF+XoZDIjd7aP6c529U4MKxGSWiUnlcrI1MVdPIwXnnqbbPVCLrddSiTSKh7fx9szLTXG1Co5iURGXkx+Gw/jhae+kxbBxVgsuQopFku5CN7srjF+cedGFWdPE/+Bh/HCc994fn5+kq+2uZlwMfZqVD3rvqbenG7WIH89Nl7FQbO4uDiJh/HSxu5V9Pf3P7asrA15YyOuEYlsqtHRcTU0NKxBTs3pw0UzMDDwCA/jpa/CXZ6ga25u7gMTrK/H5QnFDgUcuGjQGg93ee5zE1zxer2+cPjbJ5aIOBKJ/hH04ITD4c9o0BoP97m5PxBBp6DL4/H0zszMTKfTVjkaTcpk+weQy5IVvdnZ2VBbW5sfjdFW/UD4YHROahWwgOuC+z09PcNTU1PTq6uRqBj9lAmLKyuRDWp9fX0jcAwXDVo88NLGhPsnJGgXQGSKe4JewQPBQwNyavTgwOUKav6ECE6gwDYhcDpfjXtDfFtw1+CWqdGDAxcNWjzcaZ1wzDmVr8R9MQmb5hkxGSCnRg8OXGfSGlMnHHPuiSUg4vnwNnn4gJwaPThwDzWtDEiAZ4OQr4kJIKdGz+H9FnV1vwCJAvW00y66NwAAAABJRU5ErkJggg==';

.pos {
	background-image: url($pos);
}

.bottom-bar {
	width: 100%;
	height: transformRem(120);
	position: fixed;
	bottom: 0;
	left: 0;
	z-index: 50;
	&>.bom-bar-bg {
		width: 100%;
		height: 100%;
		background: -o-linear-gradient(top , rgb(51, 51, 51) 28% , rgb(30, 30, 30) 68%);
		background: -ms-linear-gradient(top , rgb(51, 51, 51) 28% , rgb(30, 30, 30) 68%);
		background: -moz-linear-gradient(top , rgb(51, 51, 51) 28% , rgb(30, 30, 30) 68%);
		background: -webkit-linear-gradient(top , rgb(51, 51, 51) 28% , rgb(30, 30, 30) 68%);
		//background: linear-gradient(top , rgb(51, 51, 51) 28% , rgb(30, 30, 30) 68%);
		display: flex;
	}
	
}

.left-part {
    float: left;
	height: transformRem(70);
	width: 70%;
	padding-left: transformRem(25);
	padding-top: transformRem(25);
	flex: 1
    &>.progress-bar {
		width: 100%;
		height: transformRem(12);
    	position: relative;
    	cursor: pointer;
    	background-color: #414141;
    	border-radius: transformRem(6);
    	&>.prog {
    		height: 100%;
    		position: absolute;
    		left: 0;
    		top: 0;
    	}
    	&>.load-prog {
    		height: 100%;
    		background-color: #535353;
    		border-radius: transformRem(6);
    	}
    	&>.play-prog {
    		background-color: #c70c0c;
    		border-radius: transformRem(6);
            width: 0;
    		&>span {
				width: transformRem(33);
				height: transformRem(36);
                float: right;
                margin-top: transformRem(-11);
                margin-right: transformRem(-16.5);
				background-position: 0 0;
				background-repeat: no-repeat;
				display: block;
				cursor: pointer;
				background-size: cover;
    		}
    	}
	}
    &>.song-msg-bar {
        width: 100%;
		height: transformRem(58);
		line-height: transformRem(76);
		&>.music-name {
			width: 65%;
			color: #fff;
			float: left;
            text-align: left;
		}
		&>.music-time {
			width: 35%;
			float: right;
			color: #797979;
			text-align: right;
			&>.ct {
				color: #a1a1a1;
			}
		}
    }
}

.right-part {
    float: right;
	height: transformRem(100);
	width: transformRem(100);
	padding-left: transformRem(25);
	padding-right: transformRem(25);
	padding-top: transformRem(10);
	//box-sizing: border-box;
}

.play-btn {
    width: 100%;
    height: 100%;
}
</style>
