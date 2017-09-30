<template>
    <div class="song-lyrics">
        <div id="lyrics-scroll" class="lyrics-scroll" v-bind:style="{transform:scollUpSize}" ref="lyricsBox">
            <p v-for="(row,index) in lyrics" v-bind:class="{ 'cur': index==lightRowIndex }">{{row[1]}}</p>
        </div>
    </div>
</template>

<script>
export default {
  	name: 'lyrics',
  	data () {
    	return {
			lightRowIndex:0,
			scollIndex:0,
			ph:0
    	}
  	},
  	props: ['lyrics','currentTime','isEnd'],
  	watch:{
		lyrics:function() {
			this.initLyricsPosition();
		},
		currentTime: function() {
			this.positionLyric();
		},
		isEnd:function(){
			if(this.isEnd) {
				this.initLyricsPosition();
			}
		}
	},
	computed:{
		scollUpSize: function() {
			return 'translateY('+(- this.scollIndex * this.ph)+'px)';
		}
	},
  	methods:{
		initLyricsPosition:function(){
			this.lightRowIndex = 0;
			this.scollIndex = 0;
			this.ph = 0;
		},
		positionLyric: function() {
			//遍历所有歌词，看哪句歌词的时间与当然时间吻合
			var indexArr = [];
			for (var i = 0; i < this.lyrics.length; i++) {
				/*是否匹配当前播放的时间*/
				if (this.currentTime  > this.lyrics[i][0]) {
				indexArr.push(i);
				};
			};
			//设置高亮行数
			if(indexArr.length > this.lightRowIndex) {
				this.lightRowIndex = indexArr[indexArr.length-1];
				this.scollIndex =  (this.lightRowIndex - 2 > 0) ? this.lightRowIndex - 2 : 0;
				this.ph = this.$refs.lyricsBox.querySelector('p').clientHeight;
			}
		}
  	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus">
.song-lyrics {
	width: transformRem(600);
	height: transformRem(300);
	position: absolute;
	left: 50%;
	top: transformRem(760);
	margin-left: transformRem(-300);
	overflow: hidden;
	&>.lyrics-scroll {
		text-align: center;
		line-height: transformRem(60);
		transition: transform .3s ease-out;
		&>p {
			font-size: transformRem(30);
			color: rgba(255, 255, 255, 0.6);
		}
		&>.cur {
			color: rgba(255, 255, 255, 1);
		}
	}
	
}
</style>
