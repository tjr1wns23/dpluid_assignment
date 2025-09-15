<template>
  <section class="contents-wrap">
    <div class="first-text-warp">
      <h2>Snap photos and share like <br>never before</h2>
    </div>
    <div class="cards-wrap">
      <div class="card" v-for="item in cards" :key="item.id">
        <img :src="item.img" :alt="item.alt">
        <p class="subheading">{{ item.subheading }}</p>
        <span class="card-text">{{ item.text }}</span>
        <p class="more-txt">LEARN MORE</p>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import img1 from '../assets/section1/Ellipse1.png';
import img2 from '../assets/section1/Ellipse2.png';
import img3 from '../assets/section1/Ellipse3.png';

export default {
  name: "Section1",
  data() {
    return {
      cards: [
        {
          id: 1,
          img: img1,
          alt: "여성 사진",
          subheading: "Sed ut perspiciatis",
          text: "Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem."
        },
        {
          id: 2,
          img: img2,
          alt: "남성 사진",
          subheading: "Lorem ipsum dolor",
          text: "Amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis."
        },
        {
          id: 3,
          img: img3,
          alt: "고양이 사진",
          subheading: "Nemo enim ipsam",
          text: "Consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor."
        }
      ]
    };
  },
  created() {
    this.shuffleCards();
  },
  methods: {
    shuffleCards() {
      for (let i = this.cards.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
      }
    }
  }
};
</script>

<style lang="css" scoped>
.contents-wrap {
  width: 1680px;
  height: 796px;
}

.first-text-warp {
  font-size: 48px;
  height: 300px;
  text-align: left;
}

.first-text-warp h2 {
  width: 750px;
  height: 144px;
  flex-grow: 0;
  font-family: 'Exo 2';
  font-size: 48px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.72px;
  text-align: left;
  color: #000;
  margin: 0;
  margin-left: 80px;
  padding-top: 120px;
}

.cards-wrap {
  display: flex;
  width: 1520px;
  margin: 34px auto 0 auto;
  gap: 20px;
  height: 352px;
}

.cards-wrap .card {
  flex: 1;
}

.cards-wrap .card .subheading {
  font-family: Montserrat;
  font-weight: 700;
  font-size: 24px;
  line-height: 36px;
  letter-spacing: -1.5%;
  margin-top: 40px;
}

.cards-wrap .card .card-text {
  font-family: Montserrat;
  font-weight: 400;
  font-size: 18px;
  line-height: 30px;
  letter-spacing: -1.5%;
  margin-top: 24px;
}

.cards-wrap .card .more-txt {
  margin: 24px 0 0;
  font-family: 'Exo 2';
  font-weight: 700;
  font-size: 18px;
  line-height: 30px;
  letter-spacing: -1.5%;
  text-transform: uppercase;
  color: #18A0FB;
}
</style>