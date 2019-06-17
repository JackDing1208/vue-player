<template>
  <div>
    <section class="state-1">
      <main>
        <div class="main-wrapper clearfix">
          <div class="left">
            <h1>晴天</h1>
            <h2>周杰伦</h2>
            <figure ref="figure"></figure>
            <div class="action">
              <svg class="icon" aria-hidden="true" v-if="!isRecycle">
                <use xlink:href="#icon-xunhuanbofang"></use>
              </svg>
              <svg class="icon " aria-hidden="true" v-if="isRecycle">
                <use xlink:href="#icon-xunhuanjiagongcishu"></use>
              </svg>
              <svg class="icon " aria-hidden="true" v-if="!isPlaying"
              @click="playSong">
                <use xlink:href="#icon-bofang"></use>
              </svg>
              <svg class="icon " aria-hidden="true" v-if="isPlaying"
              @click="pauseSong"
              >
                <use xlink:href="#icon-zantingtingzhi"></use>
              </svg>
              <svg class="icon" aria-hidden="true" @click="nextSong">
                <use xlink:href="#icon-xiayishou"></use>
              </svg>
            </div>
          </div>
          <div class="right">
            <div class="tagName">{{currentChannnelName}}</div>
            <h1>{{(currentSong)?currentSong.title:'七里香'}}</h1>
            <h2>{{(currentSong)?currentSong.artist:'周杰伦'}}</h2>
            <div class="lyrics-wrapper">
              <div class="lyrics">
                <p>周杰伦</p>
                <p>《叶惠美》</p>
                <p>故事的小黄花</p>
                <p>从出生那年就飘着</p>
                <p>童年的荡秋千</p>
                <p>随记忆一直晃到现在</p>
                <p>rui sou sou xi dou xi la</p>
                <p>sou la xi xi xi xi la xi la sou</p>
                <p>吹着前奏望着天空</p>
                <p>我想起花瓣试着掉落</p>
                <p>为你翘课的那一天</p>
                <p>花落的那一天</p>
                <p>教室的那一间</p>
                <p>我怎么看不见</p>
                <p>消失的下雨天</p>
                <p>我好想再淋一遍</p>
              </div>
            </div>

            <div class="progress">
              <div class="progressBar">
                <div class="currentBar"></div>
              </div>
              <div class="duration">-0:00</div>
            </div>
          </div>
        </div>
      </main>
      <footer>
        <svg class="icon arrow-left" aria-hidden="true" @click="leftList">
          <use xlink:href="#icon-arrow-left"></use>
        </svg>
        <div class="channel-wrapper">
          <ul class="channels " ref="channels">
            <li class="channel" v-for="value in channelList" :key="value.channel_id"
            @click="selectChannel(value.channel_id,value.name)"
            >
              <img :src="value.cover_small" alt="">
              <h3 class="title">{{value.name}}</h3>
            </li>
          </ul>
        </div>
        <svg class="icon arrow-right" aria-hidden="true" @click="rightList">
          <use xlink:href="#icon-arrow-right"></use>
        </svg>


      </footer>
    </section>
    <div class="bg"></div>
  </div>

</template>

<script>
  import '../assets/svg'
  import axios from 'axios'
  export default {
    name: "Player",
    data(){
      return{
        channelList:null,
        currentChannnel:'',
        currentChannnelName:'',
        currentSong:null,
        audio:new Audio(),
        isPlaying:false,
        isRecycle:false,
        initScroll:0
      }
    },
    created() {
      axios.get('//jirenguapi.applinzi.com/fm/getChannels.php')
        .then((res)=>{
          this.channelList=res.data.channels
          console.log(res.data.channels);
        })
    },
    computed:{
      listWidth(){
        let {width}= this.$refs.channels.getBoundingClientRect()
        return width
      }
    },
    methods:{
      selectChannel(id,name){
        console.log(id);
        this.currentChannnel=id
        this.currentChannnelName=name

        axios.get(`//jirenguapi.applinzi.com/fm/getSong.php?channel=${id}`)
          .then((res)=>{
            this.currentSong=res.data.song[0]
            console.log(this.currentSong);
            this.setCurrentSong()
          })
      },
      setCurrentSong(){
        this.audio.src=this.currentSong.url
        this.$refs.figure.style.backgroundImage=`url(${this.currentSong.picture})`
      },
      nextSong(){
        axios.get(`//jirenguapi.applinzi.com/fm/getSong.php?channel=${this.currentChannnel}`)
          .then((res)=>{
            this.currentSong=res.data.song[0]
            console.log(this.currentSong);
            this.setCurrentSong()
          })
      },
      playSong(){
        this.isPlaying=true
        this.audio.play()
        this.audio.autoplay=true
      },
      pauseSong(){
        this.isPlaying=false
        this.audio.pause()
      },
      leftList(){
        this.initScroll-=1
        if(this.initScroll<0){this.initScroll=0}
        this.$refs.channels.style.transform=`translateX(-${50 * this.initScroll}%)`
      },
      rightList(){
        this.initScroll+=1
        if(this.initScroll>19){this.initScroll=19}
        this.$refs.channels.style.transform=`translateX(-${50 * this.initScroll}%)`
      }
    }
  }
</script>

<style scoped >

</style>
