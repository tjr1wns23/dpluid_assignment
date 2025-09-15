<template>
  <section class="contents-wrap" :style="photoUrl ? {
    backgroundImage: `url(${photoUrl})`, backgroundSize: 'cover',
    backgroundPosition: 'center'
  } : {}">

    <h2 class="subheading">Sed ut perspiciatis unde omnis</h2>
    <span class="explain-txt">
      There are many variations of passages of Lorem Ipsum available, but the majority have suffered alteration in
      some form, by injected humour, or randomised words which don't look even slightly believable. If you are going to
      use a passage of Lorem Ipsum, you need to be sure there isn't anything embarrassing hidden in the middle of text.
      All the Lorem Ipsum generators on the Internet tend to repeat predefined chunks as necessary.
    </span>
    <div class="horizontal-bar"></div>
    <span class="middle-txt">
      Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem
      aperiam, eaque ipsa quae ab illo inventore.
    </span>
    <div class="subscribe-wrap">
      <div class="subscribe-box">
        <p class="subscribe-txt">Subscribe to our newsletter</p>
        <div class="form-wrap" :class="[
            email !== '' && emailIsValid ? 'form-border-valid' : '',
            email !== '' && !emailIsValid ? 'form-border-invalid' : ''
          ]">
          <input type="text" v-model="email" placeholder="Enter your email"  @input="checkEmail" @keyup.enter="sendEmail">
          <img src="../assets/section2/paper-plane1.png" alt="전송버튼" :class="!emailIsValid ? `no-btn` : ``" @click="sendEmail">
        </div>
        <span v-show="!emailIsValid&&email!=''" class="form-warning">Please enter a valid email!</span>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
export default {
  name: "section2",
  data() {
    return {
      photoUrl: null as string | null,
      email: "" as string,
      emailIsValid: false as boolean,
    };
  },
  async created() {
    const saved = localStorage.getItem("section2BgImage");
    if (saved) {
      this.photoUrl = saved;
    } else {
      await this.fetchPhoto();
    }
  },
  methods: {
    async fetchPhoto() {
      try {
        const response = await fetch(
          "https://api.unsplash.com/photos/random?client_id=RfZSbn_rdvEPrnhslq8HRwmCwyayZg3DBo_LDcXXaTM"
        );
        if (!response.ok) {
          throw new Error("API 요청 실패");
        }
        const data = await response.json();

        const url = data.urls.raw + "&w=1680&h=740&fit=crop";

        localStorage.setItem("section2BgImage", url);
        this.photoUrl = url;
      } catch (error) {
        console.error("에러 발생:", error);
      }
    },
    checkEmail() {
      // 이메일 정규식 검사
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      this.emailIsValid = re.test(this.email);
    },
    sendEmail() {
      if (this.emailIsValid) {
        alert("Thank you for subscribing");
      }
    }
  },
};
</script>

<style lang="css" scoped>
.contents-wrap {
  width: 1680px;
  height: 740px;
  text-align: center;
}

.subheading {
  width: 1520px;
  height: 36px;
  margin: 0 auto 23px auto;
  padding-top: 152px;
  font-family: Montserrat;
  font-size: 24px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.36px;
  text-align: center;
  color: #fff;
}

.explain-txt {
  display: inline-block;
  width: 1520px;
  height: 89px;
  margin: 23px 0 32px;
  font-family: Montserrat;
  font-size: 18px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.67;
  letter-spacing: -0.27px;
  text-align: center;
  color: rgba(255, 255, 255, 0.8);
}

.horizontal-bar {
  width: 1520px;
  height: 1px;
  margin: 32px auto;
  background-color: rgba(255, 255, 255, 0.5);
}

.middle-txt {
  width: 1518px;
  height: 43px;
  flex-grow: 0;
  margin: 32px 0 0 2px;
  font-family: Montserrat;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.57;
  letter-spacing: -0.21px;
  text-align: center;
  color: rgba(255, 255, 255, 0.6);
}

.subscribe-wrap {
  margin-top: 95px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.subscribe-box {
  display: flex;
  flex-direction: column;
}

.subscribe-txt {
  flex-grow: 0;
  margin: 0;
  font-family: Exo2;
  font-size: 16px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  text-align: center;
  color: #fff;
}

.form-wrap {
  display: flex;
  align-items: center;
  width: 500px;
  height: 50px;
  flex-grow: 0;
  margin: 16px 0 0;
  padding: 4px 10px 4px 4px;
  border-radius: 7px;
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  border: solid 1px #fff;
  background-color: rgba(255, 255, 255, 0.1);
}

.form-border-valid {
  border: 1px solid #00C300;
}

.form-border-invalid {
  border: 1px solid #FF6633
}

.form-wrap input {
  width: 446px;
  height: 42px;
  margin: 0 8px;
  font-family: Exo2;
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  text-align: left;
  color: #fff;
  background: none;
  border: none;
  outline: none;
}

.form-wrap input::placeholder {
  color: #fff;
}

.form-wrap img {
  width: 32px;
  height: 32px;
  flex-grow: 0;
  margin: 5px 0 5px 8px;
  object-fit: contain;
}

.form-wrap .no-btn {
  opacity: 0.5;
}

.form-warning {
  margin: 8px 0 0 14px;
  align-self: flex-start;
  font-family: Exo2;
  font-weight: 400;
  font-style: Light;
  font-size: 16px;
  line-height: 100%;
  letter-spacing: -1.5%;
  color: #FF6633;
}
</style>