<template>
<div id="surveyContainer">
  <div class="progress surveyProgress">
    <div class="progress-bar bg-info" role="progressbar" style="width: 0%" aria-valuenow="50" aria-valuemin="0" aria-valuemax="100"></div>
  </div>
  <div class="survey-wrap">
    <div class="surveys">

      <!-- <div id="loading" class="survey" v-if="this.questions==0">
        <div class="spinner-border" role="status"></div>
      </div> -->
      <div :key="index" v-for="(question, index) in questions" class="survey">
        <div class="question"><span>Q{{ index+1 }}</span> {{ question.question}}</div>
        <div class="option">
          <div :key="choice" v-for="(choice, i) in question.answer" class="container">
            <input type="radio" name='index' :id="choice" :value="i" v-model="test[index]" required>
            <label :for="choice" @click="next()">{{ choice }}</label>
          </div>
        </div>
      </div>
      <div id="surveySubmit" class="survey">
        <h4>完成所有題目了，趕快來看看結果吧！</h4>
        <div class="button">
          <input type="button" class="btn btn-info" value="再玩一次" @click="reload()">
          <input type="submit" class="btn btn-info" value="看結果" @click="submitAnswer()">
        </div>
      </div>

      <!-- loading effect -->
      <div class="modal fade" id="loading" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="false">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLongTitle">正在努力尋找最佳景點ヽ(✿ﾟ▽ﾟ)ノ(靜態Demo會一直loading))</h5>
                <span aria-hidden="true" @click="reload()">&times;</span>
            </div>
            <div class="modal-body">
              <div class="spinner-border" role="status"></div>
            </div>
          </div>
        </div>
      </div>
      <!-- loading effect -->
      <!-- results -->
      <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="false">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLongTitle">以下是測試的結果<br>打包好行李準備出門玩吧</h5>
              <button type="button" @click="close()" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <a :key="result.article_url" v-for="result in results" :href="result.article_url" target="_blank">{{ result.spot_name }}<br></a>

            </div>
            <div class="modal-footer">
              <!-- <button @click="close()" type="button" class="btn btn-secondary" data-dismiss="modal">關閉</button> -->
              <h6>點擊景點可以看相關文章喔～</h6>
            </div>
          </div>
        </div>
      </div>
      <!-- results -->
    </div>
  </div>
  <img src="https://res.cloudinary.com/tzuhsin/image/upload/v1566400988/wave_r05oq4.png" class="bgImage">
</div>
</template>

<script>
// import axios from 'axios';
export default {
  name: 'survey',
  data (){
    return{
      questions: [
        { question: '喜歡什麼顏色？', answer: ['橘色','藍色','綠色','粉色'] },
        { question: '願意嘗試浮潛嗎？', answer: ['願意','不願意'] },
        { question: '什麼動物比較吸引你？', answer: ['貓','鳥類','海洋生物'] },
        { question: '到了景點後第一件事是？', answer: ['吃吃吃！','購物','拜訪當地古蹟','拍照'] }
      ],
      test: {
        0:'0',
        1:'0',
        2:'0',
        3:'0',
        4:'0',
        5:'0',
        6:'0',
        7:'0',
      },
      mapping: ["A","B","C","D"],
      results: [],
      page: 1,
    }
  },
  // mounted() {
  //   this.question();
  // },
  methods: {
    // question () {
    //   axios({
    //     method: 'GET',
    //     url: 'http://35.193.69.171/homehome-pixnet-hackathon/public/index.php/ques'})
    //   .then((response) => {
    //     //console.log(response.data);
    //     this.questions = response.data;
    //   })
    //   .catch(function (err) {
    //       console.log(err);
    //   })
    // },
    close () {
      document.getElementById('exampleModalCenter').style.display='none';
    },
    submitAnswer () {
      document.querySelector(".surveyProgress").style.display = 'none';
      document.getElementById('loading').style.display='block';
      document.getElementById('loading').style.opacity='1';
      var ans = Object.values(this.test)
      for(var i = 0 ; i <ans.length ; i++){
        ans[i] = this.mapping[ans[i]];
      }
      ans = ans.join();
      ans = ans.replace(/,/g,'');
      console.log(ans)
      // axios({
      //   method: 'GET',
      //   url: `http://35.193.69.171/homehome-pixnet-hackathon/public/index.php/spot/${ans}`})
      // .then((response) => {
      //   //console.log(response.data);
      //   this.results = response.data;
      //   document.getElementById('loading').style.display='none';
      //   document.getElementById('loading').style.opacity='0';
      //   document.getElementById('exampleModalCenter').style.display='block';
      //   document.getElementById('exampleModalCenter').style.opacity='1';
      // })
      // .catch(function (err) {
      //   console.log(err);
      // })
    },
    next () {
      if (this.page<9) {
        document.querySelector(".surveys").style.top = "-"+this.page*100+"%";
        document.querySelector(".progress-bar").style.width = this.page*25+"%"
      }
      this.page+=1;
    },
    reload () {
      location.reload();
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import '../assets/scss/survey.scss';
</style>
