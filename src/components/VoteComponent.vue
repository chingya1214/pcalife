<template>
<div>
  <header>
    <div class="logo">
      <img src="@/assets/img/header.png" alt="">
    </div>
    <input class="menu-btn" type="checkbox" id="menu-btn" />
    <label class="menu-icon" for="menu-btn"><span class="navicon"></span></label>
    <nav>
      <ul>
        <li><a href="https://pcalifeinnovation.com.tw/#news">最新消息</a></li>
        <li><a href="https://pcalifeinnovation.com.tw/#about">關於保誠創新智造所</a></li>
        <li><a href="https://pcalifeinnovation.com.tw/#video">概念影片</a></li>
        <li><a href="https://pcalifeinnovation.com.tw/#bonus">競賽獎金</a></li>
        <li><a href="https://pcalifeinnovation.com.tw/#review">評審內容</a></li>
        <li><a href="https://pcalifeinnovation.com.tw/#join">徵件辦法</a></li>
      </ul>
    </nav>

  </header>
  <div id="vote">
    <div class="banner">
      <img src="../assets/img/banner.png" width="100%" alt="" />
      <img
        class="logoimg"
        src="../assets/img/pcalifelogo.svg"
        width="100%"
        alt=""
      />
      <div>
        <img class="diamond" src="../assets/img/red-diamond.svg" alt="" />
        <img class="diamond" src="../assets/img/blue-diamond.svg" alt="" />
        <img class="diamond" src="../assets/img/yellow-diamond.svg" alt="" />
      </div>
    </div>

    <div class="title-box">
      <h1 class="vote-title">投票專區</h1>
      <p>
        投票注意事項<br />
        投票期間：2022 年 03 月 21 日（一） 上午 10:00 ~ 2022 年 03 月 31
        日（四）中午 12:00。<br />
        本次投票採用 Facebook 臉書帳號投票，所有投票者皆需經過 Facebook
        臉書登入認證。<br />
        活動期間，每一個追夢計畫每人每天皆有一票可以表達支持，請廣邀好友踴躍投票。<br />
        為了投票的公平性，嚴格禁止灌票行為，若有發現任何單位或個人進行不當投票行為，經查證若為決選團體所為，將取消該團體參賽資格。<br />
        結果公布：整合評審面試分數（60%）+ 網路投票分數（40%），追夢得主將於
        2020 年 12 月 10 日（四） 下午 7:00 公布。<br />
        若您使用 APP（例如：LINE）打開此網站，由於 APP
        內建瀏覽器問題，可能會造成無法投票，建議您使用手機瀏覽器（例如：Google
        Chrome 或 Safari）開啟。
      </p>
    </div>
    <div v-if="loading">Loading...</div>
    <div class="groups-row" v-else>
      <div class="col-lg-6 col-12" v-for="item in groups" v-bind:key="item.id">
        <div class="group-item">
          <div class="group-title">{{ item.teamname }}</div>
          <div class="group-img">
            <img :src="item.img[0].url" width="100%" alt="" />
          </div>
          <div class="group-bottom">
            <div class="group-proposal">提案：{{ item.title }}</div>
            <a class="group-btn" href="javascript:;" @click="popopen(item.id)"
              >查看提案</a
            >
          </div>
        </div>
      </div>
      <div>
        <img class="diamond2" src="../assets/img/blue-diamond.svg" alt="" />
        <img class="diamond2" src="../assets/img/yellow-diamond.svg" alt="" />
      </div>
    </div>
    <div class="popup-wrap" v-if="popToggle">
      <div class="popup-box" v-show="popConfirmStep === 0">
        <div class="group-title">
          {{ popdata.teamname }}—{{ popdata.title }}
        </div>
        <div class="group-img">
          <img :src="popdata.img[0].url" width="100%" alt="" />
        </div>
        <div class="group-intr">{{ popdata.description }}</div>
        <!-- 投票時間檢測 -->
        <a
          v-show="checkVoteTime === true"
          class="group-btn"
          href="javascript:;"
          @click="popStepChange(1)"
          >投我一票</a
        >
        <a
          v-show="checkVoteTime === false"
          class="group-btn"
          href="javascript:;"
          @click="popclose"
          >回到提案總覽</a
        >
        <p
          v-show="checkVoteTime === false"
          style="margin-top: 10px; color: #ff3445; font-size: 14px"
        >
          投票時間為：3/21 00:00 ~ 3/31 23:59
        </p>
        <a class="close-btn" href="javascript:;" @click="popclose"></a>
      </div>
      <div class="popvote-box step1" v-show="popConfirmStep === 1">
        <h3 class="step1title"><img src="../assets/img/votestep1.png" width="100%" alt=""></h3>
        <h4>*您的Email</h4>
        <input type="text" v-model.trim="input.email" />
        <p>如果您願意參與本次抽獎，請協助填寫姓名、電話</p>
        <h4>姓名</h4>
        <input type="text" v-model.trim="input.name" />
        <h4>手機號碼</h4>
        <input type="text" v-model.trim="input.phone" />
        <a class="group-btn" href="javascript:;" @click="submitHandler"
          >送出投票</a
        >
        <p>
          <span>{{ warningtext }}</span>
        </p>
        <a class="close-btn" href="javascript:;" @click="popclose"></a>
      </div>
      <div class="popvote-box step2" v-show="popConfirmStep === 2">
        <h3 class="step2title"><img src="../assets/img/votestep2.png" width="100%" alt=""></h3>
        <h4>感謝您的投票！</h4>
        <a class="group-btn" href="javascript:;" @click="popclose"
          >回到提案總覽</a
        >
        <br /><br />
        <a class="group-btn" href="javascript:;" @click="popclose"
          >保誠創新智造所</a
        >
      </div>
    </div>
    <div v-else />
  </div>
  <footer>
    <img src="@/assets/img/robot.png" alt="">
    <ul>
      <li><a href="https://www.pcalife.com.tw/zh/disclaimer/" target="_blank">網站綜合聲明</a></li>
      <li><a href="https://www.pcalife.com.tw/zh/outsourcing-notice/" target="_blank">保險委外作業說明</a></li>
      <li><a href="https://www.pcalife.com.tw/zh/financial-friendly/" target="_blank">金融友善服務專區</a></li>
      <li><a href="https://www.pcalife.com.tw/zh/gdpr-privacy-notice/" target="_blank">GDPR隱私權告知事項</a></li>
      <li><a href="https://www.pcalife.com.tw/zh/law-advocacy/" target="_blank">法令宣導</a></li>
    </ul>
    <p>客戶服務專線 : 0809-0809-68<br>(服務時間 : 週一 ~ 週五 08:00~20:00 及週六、日及例假日 09:00~17:30)</p>
    <p>建議使用Chrome、iOS/Safari、Microsoft Edge 以取得最佳瀏覽效果</p>
    <p>保誠人壽保險股份有限公司版權所有<br>Copyright © 2022 PCA Life Assurance Co., Ltd. All rights reserved.</p>
  </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "VoteComponent",
  props: {},
  data() {
    return {
      loading: false,
      groups: [],
      input: {
        email: "",
        name: "",
        phone: "",
        workid: "",
      },
      popToggle: false,
      popConfirmStep: 0,
      workid: "",
      warningtext: "",
      checkVoteTime: false,
      checkEmailnotExis: null
    }
  },
  computed: {
    popdata() {
      return this.groups.find((item) => item.id === this.workid);
    },
  },
  mounted() {
    this.loading = true;
    axios
      .get("https://computer.bkhole.app/artworklists")
      .then((response) => {
        this.groups = response.data;
        this.groups.sort(function() {return (0.5-Math.random());});
        this.loading = false;
        // console.log(response.data);
      })
      .catch(function (error) {
        console.log(error);
      });
  },
  methods: {
    submitHandler() {
      //成功送出 checkEmailnotExis = 1 -> step2
      //Email錯誤-> step1
      //Email沒有填寫-> step1
      //已填寫過 checkEmailnotExis = 2 -> step1
      axios
        .get("https://computer.bkhole.app/votelists?email=" + this.input.email)
        .then((response) => {
          if (response.data.length > 0) {
            // console.log("response.data.length:" + response.data.length);
            this.checkEmailnotExis = 2;
          } else {
            // console.log("response.data.length:" + response.data.length);
            this.checkEmailnotExis = 1;
          }
          this.checkEmail();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    checkEmail() {
      //檢查Email格式
      let emailRegxp = /^[^\s]+@[^\s]+\.[^\s]{2,3}$/;
      if (!this.input.email || emailRegxp.test(this.input.email) == false) {
        //空值或錯誤
        this.warningtext = "＊email格式有誤，請重新填寫";
        // console.log("Email沒有提交");
        this.popStepChange(1);
      } else if (this.input.email && this.checkEmailnotExis === 1) {
        //檢查Email沒有重複
        this.input.workid = toString(this.input.workid)
        axios
          .post("https://computer.bkhole.app/votelists", this.input)
          .then(() => { //response
            // console.log(response.data);
            this.cancelHandler();
            this.popStepChange(2);
          })
          .catch(function (error) {
            console.log(error);
            this.popStepChange(1);
          });
      } else if (this.input.email && this.checkEmailnotExis !== 1) {
        //檢查Email有重複
        this.warningtext = "＊您已經投過票了，謝謝參與！";
        // console.log("Email存在");
        this.popStepChange(1);
      }
    },
    cancelHandler() {
      this.input.email = "";
      this.input.name = "";
      this.input.phone = "";
      this.input.workid = "";
    },
    popclose() {
      this.popStepChange(0);
      this.workid = "";
      this.warningtext = "";
      this.checkEmailnotExis = null;
      this.cancelHandler();
      document.body.style.overflow = "scroll";
    },
    popopen(workid) {
      this.timeCheck();
      this.popToggle = true;
      this.workid = workid;
      this.input.workid = workid.toString();
      document.body.style.overflow = "hidden";
    },
    popStepChange(step) {
      if (step === 0) {
        this.popToggle = false;
        this.popConfirmStep = step;
      } else if (step === 1) {
        this.popConfirmStep = step;
      } else if (step === 2) {
        this.popConfirmStep = step;
      }
    },
    timeCheck() {
      let voteStart = new Date(2022, 3, 21, 0, 0, 0, 0);
      let voteEnd = new Date(2022, 3, 31, 23, 59, 0, 0);
      let now = new Date(2022, 3, 22);
      // console.log(voteStart+"\n"+voteEnd+"\n"+now);
      if (now < voteStart) {
        this.checkVoteTime = false;
        console.log("投票尚未開始");
      } else if (now > voteEnd) {
        this.checkVoteTime = false;
        console.log("投票已結束");
      } else {
        this.checkVoteTime = true;
        console.log("投票中");
      }
    },
  },
};
</script>
<style scoped>
@import "../assets/css/popup.css";
  ul ,li{
    list-style: none;
    margin:0;
    padding-inline-start: 0px;
  }
  a{
    color:#000000;
    text-decoration: none !important;
  }
  header{
    display: flex;
    justify-content: flex-end;
    max-width: 1200px;
    margin: 0 auto;
    position: relative;
    margin: 32px 50px 0px 50px;
    height:50px;
    z-index: 6;
  }
  header .logo{
    /* width:360px; */
    height: 50px;
    position: absolute;
    left: 0;
    cursor: pointer;
  }
  header .logo img{
    /* width:100%; */
    height: 100%;
  }
  
  header nav ul{
      display: flex;
      font-size: 16px;
      align-items: center;
      height:100%;
  }
  header nav li{
    cursor: pointer;
    position: relative;
    font-weight: 400;
    line-height: 24px;
    padding: 13px 20px;
  }
  
  li:hover a,li a:hover{
    color:#ED1B2D;
    transition: .3s;
    transition-timing-function: cubic-bezier(.58,.3,.005,1);
  }
  header nav li:hover::after{
    content:"";
    background-color: #ED1B2D;
    bottom: 0;
    height: 3px;
    width: 100%;
    left: 0;
    display: block;
    position: absolute;
    transition: .3s;
    transition-timing-function: cubic-bezier(.58,.3,.005,1);
    box-sizing: border-box;
  }
  /* menu icon */

header .menu-icon {
  cursor: pointer;
  display: none;
  float: right;
  padding: 28px 20px;
  position: relative;
  user-select: none;

}

header .menu-icon .navicon {
  background: #333;
  display: block;
  height: 3px;
  position: relative;
  transition: background .2s ease-out;
  width: 24px;
}

header .menu-icon .navicon:before,
header .menu-icon .navicon:after {
  background: #333;
  content: '';
  display: block;
  height: 100%;
  position: absolute;
  transition: all .2s ease-out;
  width: 100%;
}

header .menu-icon .navicon:before {
  top: 8px;
}

header .menu-icon .navicon:after {
  top: -8px;
}
/* menu btn */

header .menu-btn {
  display: none;
}

header .menu-btn:checked ~ .menu-icon .navicon {
  background: transparent;
}

header .menu-btn:checked ~ .menu-icon .navicon:before {
  transform: rotate(-45deg);
}

header .menu-btn:checked ~ .menu-icon .navicon:after {
  transform: rotate(45deg);
}

header .menu-btn:checked ~ .menu-icon:not(.steps) .navicon:before,
header .menu-btn:checked ~ .menu-icon:not(.steps) .navicon:after {
  top: 0;
}

/* 48em = 768px */

/* @media (min-width: 48em) {
  header li {
    float: left;
  }
  header li a {
    padding: 20px 30px;
  }
  header .menu {
    clear: none;
    float: right;
    max-height: none;
  }
  header .menu-icon {
    display: none;
  }
} */
footer{
  background-position: top center;
  background-repeat: no-repeat;
  background-size: cover;
  padding: 0px 20px 90px 20px;
  background-image:url('~@/assets/img/footer.png');
}     
footer img{
  width: 291px;
  margin:0 auto 50px;
  display: block;
}

footer ul{
  display: flex;
  align-content: center;
  justify-content: center;
  margin-bottom: 60px;
  flex-wrap: wrap;
  font-size: 18px;
}
footer a{
  color:#fff;
}
footer li{
  font-weight: bold;
  text-decoration: underline;
  line-height: 22.5px;
  color:#fff;
  cursor: pointer;
  padding: 13px 20px;
}
footer p {
  text-align: center;
  margin-bottom:20px;
  font-size:12px;
  color:#fff;
}

#vote {
  position: relative;
  padding: 50px 207px 150px 207px;
  overflow-x: hidden;
  max-width: 1400px;
  margin: 0 auto;
  z-index: 5;
}
.banner {
  position: relative;
}
.logoimg {
  position: absolute;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.65);
}
.diamond:nth-child(1) {
  position: absolute;
  width: 30%;
  bottom: -9%;
  left: -10%;
}
.diamond:nth-child(2) {
  position: absolute;
  width: 10%;
  top: 10%;
  right: -7%;
}
.diamond:nth-child(3) {
  position: absolute;
  width: 15%;
  bottom: -30%;
  right: -15%;
}

.diamond2:nth-child(1) {
  position: absolute;
  width: 8%;
  top: 40%;
  right: -10%;
}
.diamond2:nth-child(2) {
  position: absolute;
  width: 13%;
  bottom: 10%;
  right: -15%;
}

.title-box {
  text-align: center;
}

.title-box > .vote-title {
  display: inline-block;
  position: relative;
  font-family: "Quicksand";
  font-style: normal;
  font-weight: 700;
  font-size: 72px;
  line-height: 90px;
  letter-spacing: 0.135em;
  background: linear-gradient(143.9deg, #ed1b2e 0%, #444444 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  /* text-fill-color: transparent; */
  margin: 0;
}
.title-box > p {
  font-family: "Quicksand";
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 22px;
  text-align: center;
  letter-spacing: 0.135em;
  color: #000000;
}

.vote-title::before {
  content: "";
  position: absolute;
  width: 20vw;
  height: 2px;
  background: #000000;
  top: 50%;
  left: -21vw;
}
.vote-title::after {
  content: "";
  position: absolute;
  width: 21vw;
  height: 2px;
  background: #000000;
  top: 50%;
  right: -21vw;
}
.groups-row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
  position: relative;
}

.col-lg-6,
.col-12 {
  padding: 20px 15px;
}

.group-item {
  margin: 0 auto;
  border: 2px solid #000000;
  border-radius: 30px;
  padding: 32px 39px;
  transition: 0.3s;
}
.group-item:hover {
  box-shadow: 3px 3px 12px 2px #efe6e9;
}

.group-title {
  position: relative;
  font-family: "Quicksand";
  font-style: normal;
  font-weight: 700;
  /* font-size: 24px; */
  font-size: 20px;
  line-height: 30px;
  letter-spacing: 0.135em;
  padding: 0 18px;
  margin-bottom: 20px;
  text-align: left;
}

.group-title::before {
  content: "";
  position: absolute;
  background: #ff3445;
  display: inline-block;
  height: 32px;
  width: 6px;
  top: 0;
  left: 0;
}

.group-img {
  margin: 0 auto 23px auto;
  width: 100%;
}

.group-bottom {
  display: flex;
  justify-content: space-between;
}

.group-proposal {
  display: inline-block;
  max-width: 100%;
  font-family: "Quicksand";
  font-style: normal;
  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  letter-spacing: 0.135em;
  margin: 0 10px 0 0;

  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  white-space: normal;
  height: 46px;
  overflow: hidden;
}

.group-btn {
  display: inline-block;
  background: #ff3445;
  border-radius: 15px;
  font-family: "Quicksand";
  font-weight: 700;
  /* font-size: 18px; */
  line-height: 25px;
  text-decoration: none;
  text-align: center;
  letter-spacing: 0.135em;
  color: #ffffff;
  /* padding: 18px 20px; */
  margin: 0;
  cursor: pointer;
  /* min-width: 140px; */
  transition: 0.3s;
  font-size: 16px;
  padding: 12px 20px;
  min-width: 116px;
}
.group-btn:hover {
  background: #df2f3e;
}

.group-intr {
  font-family: "Quicksand";
  font-style: normal;
  font-weight: 500;
  font-size: 20px;
  line-height: 36px;
  letter-spacing: 0.2em;
  color: #68737a;
  text-align: justify;
  margin-bottom: 20px;
}

/* desktops */
@media (max-width: 1200px) {
  #vote {
    padding: 0 120px;
  }
  header{
    min-height: 58px;
    justify-content: space-between;
  }
  header .logo{
    position: relative;
  }
  nav{
    width:100%;
    left: 0;
    position: absolute;
    top:50px;
  }
  header nav ul{
    top: 50px;
    left: 0;
    background-color: #fff;
    display: block;
    width: 100%;
    height:auto;
  }
  header nav ul li{
    width:100%;
    color: #68737A;
  }
  header nav li:hover::after{
    opacity: 0;
  }
  nav{
    max-height: 0;
    overflow: hidden;
    transition: all .2s ease-out;
  }
  header .menu-btn:checked ~ nav {
    max-height: 255px;
    transition: all .2s ease-out;
  }
  header .menu-icon{
    display: inline-block;
  }
}
@media (max-width: 992px) {
  #vote {
    padding: 0 140px;
  }
  .title-box > .vote-title {
    font-size: 55px;
    line-height: 90px;
  }
}

/* tablets */
@media (max-width: 768px) {
  #vote {
    padding: 0 100px;
  }
  header{
    margin: 32px 10px 0px 10px;
  }
  .popup-box{
    border-radius: 25px;
  }
  footer ul{
    font-size: 14px;
  }
  footer li{
    padding-top: 6px;
    padding-bottom: 6px;
  }
}

/* phones */
@media (max-width: 576px) {
  #vote {
    padding: 0 30px;
  }
  .title-box > .vote-title {
    font-size: 32px;
    line-height: 70px;
  }
  .group-bottom {
    display: block;
    text-align: center;
  }
  .group-title{
    padding-right: 40px;
  }
  .group-proposal {
    display: block;
    max-width: 100%;
    margin: 0 0 15px 0;
  }
  .group-btn {
    font-size: 16px;
  }
  .group-intr {
    font-size: 17px;
    line-height: 26px;
    letter-spacing: 0.1em;
    color: #68737a;
    text-align: justify;
    margin-bottom: 20px;
  }
}
</style>
