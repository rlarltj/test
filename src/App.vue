<template>
  <div>
    <img alt="logo" src="./assets/logo.png" class="main-logo"/>
    <div class="menu">
      <!-- login -->
      <a href="login" v-if="!login"> 로그인 </a>
      <a href="home"> Home </a>
      <router-link to="/shop" @click="card=true; banner=true; receipt=false">Shop</router-link>
      <a href="about"> About </a>
      <a v-if="login" href="home" @click="login=false"> 로그아웃 </a>
    </div>
  
<!-- @makeTrue="card=true; banner=true" :card="card" :banner="banner" -->
  <router-view @makeTrue="card=true, banner=true" @loginTrue="login=true" :login="login"></router-view>
  <div v-if="banner">
    <!-- shop 버튼을 클릭할 경우 상단 고정 Banner 등장 -->
    <Banner />
  </div>

  <div v-if="card">
    <!-- card가 true면 가격순정렬, 되돌리기 버튼과 Card Component가 화면에 렌더링됩니다.
    card의 기본값은 default며 메뉴의 shop을 클릭하면 true가 됩니다. -->
    <div class="button-Con">
      <button @click="priceSort()">가격순정렬</button>
      <button @click="sortReturn()">되돌리기</button>
    </div>
    <div class="grid">
      <Card @openModal="Modal = true; 클릭한물건= i" :물건="물건데이터[i]" v-for="(a,i) in 물건데이터.length" :key="i"/>
  </div>
  </div>


<!-- 제품을 클릭할 경우 나타나는 상세페이지(Modal)입니다.  -->
  <transition name="fade">
    <div class="black-bg" v-if="Modal">
      <div class="white-bg">
        <button @click="Modal=false" class="exitBtn"></button>
        <img :src="물건데이터[클릭한물건].image" class="modalImg">
        <h4>{{ 물건데이터[클릭한물건].title }}</h4>
        <p>{{ 물건데이터[클릭한물건].content }}</p>
        <Banner />
        <input @input="개수 = $event.target.value"  placeholder="수량을 입력하세요" >
        <p> {{개수}} 개 : {{ 물건데이터[클릭한물건].price * 개수}} 원</p>
        <h5>최소 인원: {{물건데이터[클릭한물건].min}}</h5>
        <router-link to="/cart" @click="Modal=false; card=false; banner=false; receipt=true" ><button> 입금하기!!</button></router-link>
      </div>
    </div>
  </transition>

<!-- 상세페이지에서 입금하기 버튼을 클릭하면 receipt가 true되어 receipt-container가 뜹니다. -->
    <div v-if="receipt" class="receipt-container">
      <div>
        <img :src="물건데이터[클릭한물건].image" class="receipt-image">
        <button @click="개수++">+</button>
        <button @click="개수= 개수-1">-</button>
      </div> 
      <div class="receipt-box">{{물건데이터[클릭한물건].title}}
        <div class="receipt-text">
          <div>
            {{개수}}개<br>
            총 {{ 물건데이터[클릭한물건].price * 개수}}원입니다. <br>
            <div> 구매하시겠습니까?</div>
          <button style="margin-right: 20px" @click="account=true">네</button><button @click="account=false">아니요</button>
          </div>
        </div>
      </div>
    </div>
  
  <div v-if="account">
    <h3>우리은행 1002-261-123456(김린)으로 입금해주세요!</h3>
  </div>
</div>
</template>

<script>
import Banner from './Banner.vue'
import Card from './Card.vue'

import { initializeApp } from "firebase/app";
import { getDatabase, ref, child, get } from "firebase/database";


const firebaseConfig = {
    apiKey: "AIzaSyBbUlbvrvV2Uj-LR7bDHF0RJdYy5mSNqJw",
    authDomain: "nyangdiz-b0211.firebaseapp.com",
    databaseURL: "https://nyangdiz-b0211-default-rtdb.firebaseio.com",
    projectId: "nyangdiz-b0211",
    storageBucket: "nyangdiz-b0211.appspot.com",
    messagingSenderId: "69242287095",
    appId: "1:69242287095:web:8cc8fe4af4156f706cb4f2"
  };
  
initializeApp(firebaseConfig);



// import Modal from './Modal.vue'
export default {
  name: 'App',
  data(){                     
    return{
      login:false,
      개수: 1,
      클릭한물건: 0,
      물건데이터: [],
      물건데이터2: [],
      Modal: false,
      card: false,
      banner: false,
      receipt: false,
      account: false,
    }
  },
  created () {
    this.$nextTick(async function () {
      console.log("hi2");
      const dbRef = ref(getDatabase());
      await get(child(dbRef, `goods`)).then((snapshot) => {
        if (snapshot.exists()) {
          console.log(snapshot.val());
          this.물건데이터 = snapshot.val(); 
          this.물건데이터2 = [...snapshot.val()]         
        } else {
          console.log("No data available");
        }
        }).catch((error) => {
          console.error(error);
        });
      console.log("dat");
      console.log(this.물건데이터);
      })
  },
  watch :{
    개수(a){
       if (isNaN(a) == true){
        alert('숫자를 입력하세요');
        this.개수 = 1;
       }
        else if(a<1){
          alert('한개부터 구매 가능합니다.');
          this.개수 = 1;
        }
    }
  },
  methods : {
    priceSort(){
    this.물건데이터.sort(function(a,b){
      return a.price - b.price
    })
  },  
    sortReturn(){
      this.물건데이터 = [...this.물건데이터2];
  },

  },
  components: {
    Banner: Banner,
    Card: Card,
  }
}
</script>

<style>
@font-face {
  font-family: 'Y_Spotlight';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts-20-12@1.0/Y_Spotlight.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

.main-logo{
  height: 250px;
  margin: 0 0 50px 0;
}
.menu {
  background : rgba(0,0,0,0.09);
  padding : 15px;
  border-radius : 5px;
  
}
.menu a {
 color:#05499f;
  padding: 0 15px 0 15px;
  margin: 0 2rem 0 2rem;
  font-weight: bold;
  font-size: 20px;
}
#app {
  font-family: 'Y_Spotlight', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-bottom: 20px;
}

body {
  margin : 0;
}
div {
  box-sizing: border-box;
}
.button-Con {
  display: flex;
  justify-content: right;
}
.modalImg{
  width: 60%;
  height: 400px;
}
.black-bg {
  width: 100%; height:100%;
  background: rgba(0,0,0,0.5);
  position: fixed;
  top:0;
  padding: 20px;
}
.white-bg {
  margin: auto;
  position: relative;
  width: 50%; background: white;
  border-radius: 8px;
  padding: 20px;
} 
.exitBtn{
  position: absolute;
  width: 40px;
  height: 40px;
  display:inline-block;*display:inline;
  right: 5px;
  top: 5px;
  font-weight: bold;
}
.exitBtn:after {display: inline-block;content: "\00d7"; font-size:15pt;}
.room-img{
  float: clear;
  width: 80%;
  margin-top: 40px;
}
.button-Con{
  padding-right:10px;
}
.button-Con button{
  margin-left: 10px;
}
.banner{
  background: rgba(46,110,185,0.35);
  padding: 10px;
  margin: 10px;
  border-radius: 10px;
  height: 100%;
}

.fade-enter-from{
  opacity: 0;
}
.fade-enter-active{
  transition: all 1s;
}
.fade-enter-to{
  opacity:1;
}

.fade-leave-from{
  opacity: 1;
}
.fade-leave-active{
  transition: all 1s;
}
.fade-leave-to{
  opacity:0;
}

/* 영수증페이지 */
.receipt-container{
  display: flex;
  flex-direction: row;
  justify-content: center;
  text-align: center;
  padding: 3rem;
  font-size: 2rem;
}
.receipt-image {
  max-width: 100%;
}
.receipt-box{
  display: flex;
  flex-direction: column;
  text-align:center;
  justify-content: center;
}

.receipt-text{
  color: #132452; 
  font-weight: bold;
  font-weight: bold;
  margin-top: 3rem;
  padding: 0 2rem;
}
</style>
