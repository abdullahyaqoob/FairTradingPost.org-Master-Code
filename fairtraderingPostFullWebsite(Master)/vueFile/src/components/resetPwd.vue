<template>
  <div class="authMainDiv">
    <div class="authDiv">
      <!-- <h2>Thank you for applying as a Mediator</h2> -->

      <!-- <p style="text-align:center;"><img src="../assets/img/topLogoTxt.png" alt="" class="topLogoPic"></p> -->
      <p style="text-align: center">
        <img
          src="../assets/img/bottomLogoTxt (1).png"
          alt=""
          class="topLogoPic"
        />
      </p>
      <div class="form">
        <!-- <h3>Sign In / Sign Up</h3> -->
        <h3>Reset Your Account Password</h3>

        <!-- <div class="row">
          <div class="col-6">
            <span class="inputLabel">First Name <small>(Optional)</small></span><br />
            <input
              class="inputFeild"
              type="text"
              placeholder="Enter Your Name ..."
            />
          </div>
          <div class="col-6">
            <span class="inputLabel">Last Name <small>(Optional)</small></span><br />
            <input
              class="inputFeild"
              type="text"
              placeholder="Enter Your Name ..."
            />
          </div>
        </div> -->
        <br />
        <span class="inputLabel">Your Email</span><br />
        <input
          class="inputFeild"
          style="width: 98%; cursor: not-allowed"
          type="text"
          disabled
          :value="userEmail"
          placeholder="Enter Your Email ..."
        />
        <br />
        <br />
        <div class="row">
          <div class="col-6">
            <span class="inputLabel">Your Password</span><br />
            <input
              class="inputFeild"
              style="width: 98%"
              type="password"
              placeholder="Enter Your password here ..."
              v-model="emailPassword1"
            />
          </div>
          <div class="col-6">
            <span class="inputLabel">Confirm Password</span><br />
            <input
              class="inputFeild"
              style="width: 98%"
              type="password"
              placeholder="Confirm Your Password ..."
              v-model="emailPassword2"
            />
          </div>
        </div>

        <p class="applyDIv">
          <!-- <span class="btnTopTxt">first time application</span> -->
          <button
            v-if="!loading"
            class="authBtn"
            @click="handleSubmit()"
            style="background-color: #ed1b23"
          >
            Reset Password
          </button>
          <button
            v-if="loading"
            class="authBtn"
            style="background-color: #ed1b23"
          >
            <img
              src="@/assets/img/loading.gif"
              style="vertical-align: sub; width: 17px"
            />
            <span class="loading-text">Loading...</span>
          </button>
          <!-- <span class="btnTopTxt">to complete application</span> -->
          <!-- <a href="/apply"
            ><button
              v-if="!loading"
              @click="handleLogin()"
              class="authBtn"
              style="background-color: #1472e1"
            >
              Apply
            </button></a
          > -->
        </p>

        <!-- <p>
          <button
            v-if="!loading"
            class="authBtn"
            style="background-color: #0ca550"
            @click="handleSubmit()"
          >
            Sign In
          </button>
          <button v-else class="authBtn" style="background-color: #0ca550">
            <img
              src="@/assets/img/loading.gif"
              style="vertical-align: sub; width: 17px"
            />
            <span class="loading-text">Loading...</span>
          </button>
          <a href="/apply"
            ><button class="authBtn" style="background-color: #ed1b23">
              Apply
            </button></a
          >
        </p> -->
      </div>
      <!-- <p style="text-align: center">
        <img
          src="../assets/img/joinTelegram.png"
          width="200px"
          alt="authBottomPic"
          class="authBottomPic2"
        />
      </p>
      <img
        src="../assets/img/bottomLogo.png"
        width="200px"
        alt="authBottomPic"
        class="authBottomPic"
      /> -->
      <!-- <p style="text-align:end;"> -->
      <img
        src="../assets/img/bottomLogoTxt (3).png"
        alt=""
        class="bottomLogoTxt"
      />
      <!-- </p> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import AOS from "aos";
export default {
  name: "mediation",
  data() {
    return {
      userEmail: "",
      userToken: "",
      emailPassword1: "",
      emailPassword2: "",
      loading: false,
    };
  },
  mounted() {
    this.userEmail = this.$route.query.email;
    this.userToken = this.$route.query.token;
  },
  methods: {
    handleSubmit() {
      if (this.emailPassword1 === "") {
        this.$toasted.error("Invalid Email Address");
      } else if (!this.emailPassword1.match(this.emailPassword2)) {
        this.$toasted.error("Password Not Matched");
      } else {
        this.loading = true;
        axios
          .post(
            process.env.VUE_APP_SERVICE_URL + "ResetPassword/Change",
            {
              token: this.userToken,
              email: this.userEmail,
              password1: this.emailPassword1,
              password2: this.emailPassword2,
            },
            { headers: { "Content-Type": "application/json" } }
          )
          .then((res) => {
            console.log(res);
            this.$toasted.success("Password successfully changed");
            this.$router.push("/signin");
            this.loading = false;
          })
          .catch((Response) => {
            console.log("eerrrooor", Response);
            this.$toasted.error("Password not changed");
            this.loading = false;
          });
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
.authMainDiv {
  background-color: #f9c602;
  width: 100%;
  height: 100vh;
  margin: 0 auto;
}
.authDiv {
  width: 50%;
  margin: 0 auto;
}
.authDiv h2 {
  margin-left: -40px;
  padding-top: 140px;
  font-weight: 1000;
  font-size: 40px;
  text-align: center;
  font-family: nunito;
  margin-bottom: 80px;
}
.applyDIv {
  text-align: right;
}
.authDiv h3 {
  font-size: 30px;
  color: red;
  font-weight: bold;
  /* font-family: nunito; */
  margin-bottom: 30px;
}
.inputLabel {
  font-weight: bold;
}
.btnTopTxt {
  display: none;
}
.inputFeild {
  margin-top: 10px;
  width: 95%;
  padding-left: 20px;
  height: 30px;
  font-weight: bold;
  outline: none;
}
.topLogoPic {
  margin-top: 80px;
  width: 50%;
  margin-bottom: 50px;
}
.bottomLogoTxt {
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
  margin-top: 200px;
  width: 270px;
}
.authBtn {
  border: none;
  color: white;
  font-weight: bold;
  margin-right: 8px;
  margin-left: 8px;
  padding: 9px 60px;
  margin-top: 50px;
  border-radius: 5px;
  font-size: 18px;
}
.authBottomPic {
  position: absolute;
  left: 50%;
  bottom: 5%;
  transform: translate(-50%, 0);
}

.forgotTxt {
  text-align: center;
  margin-top: 130px;
}
.authBottomPic2 {
  width: 200px;
  position: absolute;
  bottom: 17%;
  left: 50%;
  transform: translate(-50%, 0);
}
.forgotEmail {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  backdrop-filter: blur(10px);
  background: transparent;
  z-index: 98;
}
.forgotEmailDiv {
  position: absolute;
  top: 30%;
  left: 50%;
  transform: translate(-50%, 0);
  background-color: #0d54ae;
  width: 60%;
  height: 320px;
  padding: 30px 30px;
}
.forgotEmailDiv h5 {
  font-size: 20px;
  /* text-align: center; */
  padding-left: 25px;
  margin-top: 10px;
  color: white;
  font-weight: bold;
}
.forgotEmailDiv input {
  width: 90%;
  margin: 0 auto;
  margin-top: 30px;
  padding-left: 20px;
  height: 30px;
  border-radius: 5px;
  border: 1px solid lightslategrey;
}
@media (max-width: 1440px) {
  .authDiv h2 {
    margin-left: 0px;
  }
}
@media (max-width: 1200px) {
  .authDiv {
    width: 91%;
  }
}
@media (max-width: 1170px) {
  .forgotEmailDiv {
    width: 60%;
  }
}
@media (max-width: 850px) {
  .authDiv h2 {
    padding-top: 80px;
    font-size: 40px;
  }
  .inputFeild {
    font-size: 12px;
  }
  .authBottomPic {
    width: 270px;
  }
  .forgotEmailDiv {
    width: 90%;
  }
}
@media (min-width: 500px) {
  .authBtn {
    float: right;
  }
}
@media (max-width: 500px) {
  .applyDIv {
    text-align: center;
  }

  .authDiv h3 {
    font-size: 25px;
  }
  .bottomLogoTxt {
    margin-top: 100px;
    width: 50%;
  }
  .authDiv h2 {
    font-size: 32px;
    padding-top: 50px;
    margin-bottom: 50px;
  }
  .topLogoPic {
    margin-top: 30px;
    width: 70%;
    margin-bottom: 50px;
  }
  .inputFeild {
    margin-top: 4px;
  }
  .authBottomPic {
    position: relative;
    top: 140px;
    /* bottom: -150px; */
  }
  .authBottomPic2 {
    width: 180px;
    position: absolute;
    bottom: 130px;
    left: 50%;
    transform: translate(-50%, 0);
  }
  .authMainDiv {
    height: 100vh;
  }
  .authBtn {
    margin-top: 7vh;
  }
  .btnTopTxt {
    display: initial;
    margin-left: 15px;
    position: absolute;
    margin-top: 22px;
    color: #352e2e;
  }
  .forgotTxt {
    text-align: center;
    margin-top: 40px;
  }
}
@media (max-width: 430px) {
  .inputFeild {
    font-size: 11px;
    padding-left: 9px;
  }
}
@media (max-width: 419px) {
  .authDiv h2 {
    font-size: 32px;
  }
  .authBtn {
    padding: 9px 45px;
  }
}
@media (max-width: 407px) {
  .authBtn {
    padding: 9px 50px;
  }
}
@media (max-width: 400px) {
  .authBtn {
    padding: 9px 53px;
  }
  .btnTopTxt {
    margin-left: 5px;
  }
  .authBtn {
    padding: 9px 47px;
  }
  .authDiv h3 {
    font-size: 22px;
  }
}
@media (max-width: 376px) {
  .inputFeild {
    font-size: 10px;
  }
  .authBtn {
    padding: 9px 47px;
  }
  .btnTopTxt {
    margin-left: 0px;
  }
}
@media (max-width: 363px) {
  .authDiv h2 {
    font-size: 28px;
  }
  .authBtn {
    padding: 8px 40px;
  }
}
@media (max-width: 348px) {
  .authBtn {
    padding: 8px 36px;
  }
  .authDiv h3 {
    font-size: 20px;
  }
}
@media (max-width: 317px) {
  .authDiv h2 {
    font-size: 25px;
  }
}

@media (max-height: 660px) {
  .form h3 {
    font-size: 26px;
  }
  .bottomLogoTxt {
    width: 40%;
    margin-top: 80px;
  }
}
</style>