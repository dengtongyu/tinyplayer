<template>
  <div id="app">

    <disc v-bind:singer-image="singerImage" v-bind:is-play="isPlay" v-bind:hash="hash"></disc>
    <lyrics v-bind:lyrics="lyrics" v-bind:current-time="currentTime" v-bind:is-end="isEnd"></lyrics>
    <control v-bind:is-play="isPlay" v-bind:song-name="songName" v-bind:singer="singer" v-bind:current-time="currentTime" v-bind:duration="duration" v-on:clickPlayBtn="clickPlayBtn"></control>
    <div class="search-btn">
      <button v-on:touchend="showSearchPanel = !showSearchPanel">
        <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Capa_1" x="0px" y="0px" viewBox="0 0 53.627 53.627" style="enable-background:new 0 0 53.627 53.627;" xml:space="preserve" width="100%" height="100%">
          <path d="M53.627,49.385L37.795,33.553C40.423,30.046,42,25.709,42,21C42,9.42,32.58,0,21,0S0,9.42,0,21s9.42,21,21,21  c4.709,0,9.046-1.577,12.553-4.205l15.832,15.832L53.627,49.385z M2,21C2,10.523,10.523,2,21,2s19,8.523,19,19s-8.523,19-19,19  S2,31.477,2,21z" fill="#FFFFFF"/>
          </svg>
      </button>
    </div>
    <search-panel v-bind:show-search-panel="showSearchPanel" v-bind:search-song-list="searchSongList" v-on:hideSearchPanel="closeSearchPanel" v-on:searchKeyword="requestKeyword" v-on:getPlaySong="getPlaySong"></search-panel>

  </div>
</template>

<script>
import Disc from './components/Disc'
import Control from './components/Control'
import Lyrics from './components/Lyrics'
import SearchPanel from './components/SearchPanel'

export default {
  name: 'app',
  components: {
    Disc,
    Lyrics,
    Control,
    SearchPanel
  },
  data () {
    return {
      showSearchPanel:false,
      searchSongList:[],
      singerImage:'http://node.dengtongyu.com/res/images/default/default_img_600.png',
      lyrics:[],
      songName:'未知',
      singer:'未知',
      audio:null,
      isPlay:false,
      currentTime:0,
      duration:0,
      hash:0,
      isEnd:false
    }
  },
  methods: {
    closeSearchPanel:function(){
      this.showSearchPanel = false;
    },
    clickPlayBtn:function(){
      this.isPlay = !this.isPlay;
      this.setPalayStatus(this.isPlay);
    },
    requestKeyword:function(keyword){
      var url = 'http://songsearch.kugou.com/song_search_v2?keyword='+keyword+'&page=1&pagesize=30&userid=-1&clientver=&platform=WebFilter&tag=em&filter=2&iscorrection=1&privilege_filter=0&_='+Date.now();
      this.$http.jsonp(url, {
        params:{
          }
      }).then(function(json){
        //var result = json.json();
        var result = JSON.parse(json.bodyText);
        if (result) {
          
          if(result.status == 1 && result.data.lists.length > 0) {
            this.searchSongList = result.data.lists;
          }
          
          }else{
            
          }
      }, function(){
      });
    },
    getPlaySong:function(hash){
      this.$http.get('/data/songMsg?hash='+hash, {
        params:{
          }
      }).then(function(json){
        var result = JSON.parse(json.bodyText);
        if (result) {
          if(result.result) {
            this.hash = hash;

            this.singerImage = result.data.img;
            this.lyrics = this.parseLyric(result.data.lyrics);
            this.songName = result.data.song_name;
            this.singer = result.data.author_name;
            
            this.clearAudio();
            this.newAudio(result.data.play_url);
            // var songMsg = {
            //   url:result.data.play_url,
            //   name:result.data.song_name,
            //   singer:result.data.author_name,
            //   photo:result.data.img,
            //   cover:result.data.img,
            //   lyrics:this.parseLyric(result.data.lyrics)
            // }
          }
          
          }else{
          }
      }, function(){
      });
    },
    newAudio:function(url){
      this.audio = new Audio();
      //this.audio.loop = 'loop';
      this.audio.src = url;
      this.audio.load();
      this.audio.addEventListener("timeupdate", ()=>{ this.onTimeUpdate(event) });
      this.audio.addEventListener("canplaythrough", ()=>{ this.onCanPlay(event) });
      this.audio.addEventListener("ended", ()=>{ this.onEnded(event) });
      //this.audio.addEventListener("error", ()=>{ this.onError(event) });
    },
    clearAudio:function() {
      if(!this.audio) return;
      this.audioPause();
      this.audio.removeEventListener("timeupdate",  ()=>{ this.onTimeUpdate(event) });
      this.audio.removeEventListener("canplaythrough", ()=>{ this.onCanPlay(event) });
      this.audio.removeEventListener("ended", ()=>{ this.onEnded(event) });
      //this.audio.removeEventListener("error", ()=>{ this.onError(event) });
      this.audio = null;
    },
    onTimeUpdate:function(event) {
      if(!this.audio) return;
      if(this.audio.currentTime > 0) {
        this.isEnd = false;
        this.currentTime = this.audio.currentTime,
        this.duration = this.audio.currentTime != 0?this.audio.duration:0
      }
    },
    onCanPlay:function(event) {
      if(!this.audio) return;
      this.setPalayStatus(true);
    },
    onEnded:function(){
      this.isPlay = false;
      this.isEnd = true;
    },
    setPalayStatus:function(playStatus) {
      if(!this.audio.error) {
        this.isPlay = playStatus;
        if(this.isPlay) {
          this.audioPlay();
        }else {
          this.audioPause()
        }
      }else {
        alert(this._music.error.code);
        console.log(this._music.error);
      }
    },
    isWeixin:function(){ 
      var ua = navigator.userAgent.toLowerCase(); 
      if(ua.match(/MicroMessenger/i)) { 
        return true; 
      } else { 
        return false; 
      } 
    },
    audioPlay:function() {
      var audio = this.audio;
      if(this.isWeixin()) {
        wx.config({
          // 配置信息
        });
        wx.ready(function () {
          //console.log("微信浏览器准备好了");
          audio.play();
        });
      }else{
        audio.play();
      }
    },
    audioPause:function() {
      var audio = this.audio;
      if(this.isWeixin()) {
        wx.config({
          // 配置信息
        });
        wx.ready(function () {
          //console.log("点击暂停了");
          //console.log(audio.networkState);
          audio.pause();
        });
      }else{
        audio.pause();
      }
    },
    parseLyric:function(text){
      //将文本分隔成一行一行，存入数组
      var lines = text.split('\n'),
          //用于匹配时间的正则表达式，匹配的结果类似[xx:xx.xx]
          pattern = /\[\d{2}:\d{2}.\d{2}\]/g,
          //保存最终结果的数组
          result = [];
      //去掉不含时间的行
      while (!pattern.test(lines[0])) {
          lines = lines.slice(1);
      };
      //上面用'\n'生成生成数组时，结果中最后一个为空元素，这里将去掉
      lines[lines.length - 1].length === 0 && lines.pop();
      lines.forEach(function(v /*数组元素值*/ , i /*元素索引*/ , a /*数组本身*/ ) {
          //提取出时间[xx:xx.xx]
          var time = v.match(pattern),
              //提取歌词
              value = v.replace(pattern, '');
          //因为一行里面可能有多个时间，所以time有可能是[xx:xx.xx][xx:xx.xx][xx:xx.xx]的形式，需要进一步分隔
          time.forEach(function(v1, i1, a1) {
              //去掉时间里的中括号得到xx:xx.xx
              var t = v1.slice(1, -1).split(':');
              //将结果压入最终数组
              result.push([parseInt(t[0], 10) * 60 + parseFloat(t[1]), value]);
          });
      });
      //最后将结果数组中的元素按时间大小排序，以便保存之后正常显示歌词
      result.sort(function(a, b) {
          return a[0] - b[0];
      });
      return result;
    }
  }
}

</script>

<style lang="stylus">
$norm=750;
transformRem(v)
    (v/$norm*10)rem

/*全局样式*/
*{margin:0; padding:0;}
table{border-collapse:collapse; border-spacing:0;}
fieldset,img{border:0;}
address,caption,cite,code,dfn,em,var,i{font-style:normal; font-weight:normal;}
ul,ol,li{list-style:none;}
h1,h2,h3,h4,h5,h6{font-weight:normal;}
html {font-size: 62.5%}
input,textarea {outline:none;resize:none;}
pre,p{ white-space: -webkit-pre-wrap;white-space: -moz-pre-wrap; white-space: pre-wrap; word-wrap: break-word;}
a,button {-webkit-tap-highlight-color: rgba(255, 255, 255, 0);-webkit-focus-ring-color: rgba(0, 0, 0, 0);outline: none;border: none;text-decoration:none;}
a:hover{ }
body {font-size:1.2rem;font-family:"Microsoft Yahei";color:#666;background-color: #fff}
html,body{
	height: 100%;
}
.ell {overflow:hidden; white-space:nowrap; text-overflow:ellipsis;}

[data-dpr="2"] div {
    font-size: 24px;
}

[data-dpr="3"] div {
    font-size: 36px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

.search-btn {
	position: absolute;
	right: transformRem(40);
	top: transformRem(50);
	&>button {
    width: transformRem(60);
    height: transformRem(60);
		color: #fff;
		background-color: rgba(0, 0, 0, 0);
    border: none;
	}
}
</style>
