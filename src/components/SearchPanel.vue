<template>
<transition name="slide-fade">
<div class="search-pop" v-show="showSearchPanel">
    <div class="pop-bg"></div>
    <div class="pop-main">
        <div class="search-bar">
            <input class="search-input" type="text" v-model="keyword">
            <button class="search-button" v-on:touchend="searchKeyword">搜索</button>
        </div>
        <div class="search-list">
            <div class="search-list-scroll">
                <div class="search-list-item" v-for="item in searchSongList" v-on:touchend="getPlaySongHash(item.FileHash)">
                        <p class="ell">{{item.FileName | htmlEncode}}</p>
                        <button></button>
                </div>
            </div>
        </div>
        <div class="placeholder">
            <button v-on:touchend="closeSearchPanel">
                <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Capa_1" x="0px" y="0px" viewBox="0 0 51.976 51.976" style="enable-background:new 0 0 51.976 51.976;" xml:space="preserve" width="100%" height="100%">
                    <g>
                        <path d="M44.373,7.603c-10.137-10.137-26.632-10.138-36.77,0c-10.138,10.138-10.137,26.632,0,36.77s26.632,10.138,36.77,0   C54.51,34.235,54.51,17.74,44.373,7.603z M36.241,36.241c-0.781,0.781-2.047,0.781-2.828,0l-7.425-7.425l-7.778,7.778   c-0.781,0.781-2.047,0.781-2.828,0c-0.781-0.781-0.781-2.047,0-2.828l7.778-7.778l-7.425-7.425c-0.781-0.781-0.781-2.048,0-2.828   c0.781-0.781,2.047-0.781,2.828,0l7.425,7.425l7.071-7.071c0.781-0.781,2.047-0.781,2.828,0c0.781,0.781,0.781,2.047,0,2.828   l-7.071,7.071l7.425,7.425C37.022,34.194,37.022,35.46,36.241,36.241z" fill="#FFFFFF"/>
                    </g>
                </svg>
            </button>
        </div>
    </div>
</div>
</transition>
</template>

<script>
export default {
    name: 'search-panel',
    data () {
        return {
            keyword:''
        }
    },
    props: ['showSearchPanel','searchSongList'],
    methods: {
        closeSearchPanel:function(){
             this.$emit('hideSearchPanel');
        },
        searchKeyword:function(){
            this.$emit('searchKeyword',this.keyword);
        },
        getPlaySongHash:function(hash){
            this.closeSearchPanel();
            this.$emit('getPlaySong',hash);
        }
    },
	filters:{
		htmlEncode:function(str){
			var temp = document.createElement("div"); 
		    temp.innerHTML = str; 
		    var output = temp.innerText || temp.textContent; 
		    temp = null; 
		    return output;
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus">
$norm=750;
transformRem(v)
    (v/$norm*10)rem

$player = 'data:image/svg+xml;utf8;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pgo8IS0tIEdlbmVyYXRvcjogQWRvYmUgSWxsdXN0cmF0b3IgMTguMS4xLCBTVkcgRXhwb3J0IFBsdWctSW4gLiBTVkcgVmVyc2lvbjogNi4wMCBCdWlsZCAwKSAgLS0+CjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgdmVyc2lvbj0iMS4xIiBpZD0iQ2FwYV8xIiB4PSIwcHgiIHk9IjBweCIgdmlld0JveD0iMCAwIDIzMi4xNTMgMjMyLjE1MyIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMjMyLjE1MyAyMzIuMTUzOyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSIgd2lkdGg9IjEyOHB4IiBoZWlnaHQ9IjEyOHB4Ij4KPGcgaWQ9IlBsYXkiPgoJPHBhdGggc3R5bGU9ImZpbGwtcnVsZTpldmVub2RkO2NsaXAtcnVsZTpldmVub2RkOyIgZD0iTTIwMy43OTEsOTkuNjI4TDQ5LjMwNywyLjI5NGMtNC41NjctMi43MTktMTAuMjM4LTIuMjY2LTE0LjUyMS0yLjI2NiAgIGMtMTcuMTMyLDAtMTcuMDU2LDEzLjIyNy0xNy4wNTYsMTYuNTc4djE5OC45NGMwLDIuODMzLTAuMDc1LDE2LjU3OSwxNy4wNTYsMTYuNTc5YzQuMjgzLDAsOS45NTUsMC40NTEsMTQuNTIxLTIuMjY3ICAgbDE1NC40ODMtOTcuMzMzYzEyLjY4LTcuNTQ1LDEwLjQ4OS0xNi40NDksMTAuNDg5LTE2LjQ0OVMyMTYuNDcxLDEwNy4xNzIsMjAzLjc5MSw5OS42Mjh6IiBmaWxsPSIjRkZGRkZGIi8+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPGc+CjwvZz4KPC9zdmc+Cg==';

.slide-fade-enter-active {
  transition: all .5s ease;
}
.slide-fade-leave-active {
  transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active 在低于 2.1.8 版本中 */ {
  transform: translateX(10px);
  opacity: 0;
}

.search-pop {
	width:100%;
	height: 100%;
	position: fixed;
	top: 0;
	left: 0;
	z-index: 100;
    //transform: translateX(-100%);
	&>.pop-bg {
		width:100%;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		opacity: .4;
		background-color: #000;
	}
	&>.pop-main {
		width:100%;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}
}

.search-bar {
	width: transformRem(600);
	height: transformRem(50);
	padding: transformRem(30) 0;
	&>.search-input {
		width: transformRem(500);
		height: transformRem(50);
		border: rgba(255, 255, 255, 0.6) solid transformRem(2);
		border-right: none;
		background-color: rgba(0, 0, 0, 0.4);
		line-height: transformRem(50);
		color: #fff;
		font-size: transformRem(28);
		box-sizing:border-box;
		border-top-left-radius: transformRem(25);
		border-bottom-left-radius: transformRem(25);
		padding: 0 transformRem(25);
		float: left;
	}
	&>.search-button {
		width: transformRem(100);
		height: transformRem(50);
		border: rgba(255, 255, 255, 0.6) solid transformRem(2);
		background-color: rgba(0, 0, 0, 0.4);
		line-height: transformRem(50);
		color: #fff;
		font-size: transformRem(28);
		box-sizing:border-box;
		border-top-right-radius: transformRem(25);
		border-bottom-right-radius: transformRem(25);
		float: left;
	}
}

.search-list {
	width: transformRem(600);
	flex:1;
	overflow-y: auto;
	&>.search-list-scroll {
		&>.search-list-item {
			height: transformRem(50);
			padding: transformRem(10) 0;
			display: flex;
			justify-content:space-between;
			&>p {
				line-height: transformRem(50);
				width: transformRem(480);
				font-size: transformRem(26);
				color: #fff;
                text-align: left;
			}
			&>button {
				width: transformRem(30);
				height: transformRem(30);
				margin-top: transformRem(10);
				//border: rgba(255, 255, 255, 0.6) solid transformRem(2);
				border: none;
				background-image: url($player);
				background-size: cover;
				background-color: rgba(0, 0, 0, 0);
				//line-height: transformRem(50);
				color: #fff;
				//font-size: transformRem(28);
				//box-sizing:border-box;
			}
		}
	} 
}

.placeholder {
	width: transformRem(600);
	height: transformRem(100);
	display: flex;
    justify-content: center;
    align-items: center
	&>button {
		width: transformRem(70);
		height: transformRem(70);
		//border: rgba(255, 255, 255, 0.6) solid transformRem(2);
		border: none;
		background-color: rgba(0, 0, 0, 0);
		//line-height: transformRem(60);
		color: #fff;
		//font-size: transformRem(30);
		//box-sizing:border-box;
	}
}
</style>
