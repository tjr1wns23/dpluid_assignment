<template>
  <section class="contents-wrap">
    <h2 class="subheading">Duis tincidunt ut ligula vitae mollis.</h2>
    <div class="filter-wrap">
      <ul class="region-filter">
        <li v-for="region in regions" :key="region" :class="{ active: selectedRegion === region }"
          @click="selectedRegion = region">
          {{ region }}
        </li>
      </ul>
      <div class="year-filter-wrap">
        <ul class="year-filter">
          <li v-for="year in yearOptions" :key="year" :class="{
            selected: selectedYearRange.includes(year),
            inRange: isInRange(year)
          }" @click="selectYear(year)">
            {{ year }}
          </li>
        </ul>
        <div class="year-filter-active" :style="gradientStyle"></div>
      </div>

    </div>
    <div v-show="selectedYearRange.length === 2" class="cards-wrap">

      <div class="info-card" v-for="card in filteredCards" :key="card.id">
        <div class="card-header-wrap">
          <p class="title">{{ card.title }}</p>
          <p class="year">{{ card.year }}</p>
        </div>

        <img :src="card.img" :alt="card.alt" />
        <span class="info-detail">{{ card.text }}</span>
      </div>

    </div>
    <div v-show="selectedYearRange.length === 1" class="finding-wrap">
      <div class="no-card">
        <img src="../assets/section3/finding_icon.svg" alt="find icon img">
        <p class="">Please finalize your period selection</p>
      </div>
    </div>
    <div v-show="filteredCards.length === 0 && selectedYearRange.length === 2" class="no-cards-wrap">
      <div class="no-card">
        <img src="../assets/section3/finding_icon.svg" alt="find icon img">
        <p class="">No search results</p>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import img1 from '../assets/section3/Italy_Pica.jpg';
import img2 from '../assets/section3/Spain_Sagrada.jpg';
import img3 from '../assets/section3/US_Fallingwater.jpg';
import img4 from "../assets/section3/Russia_Basil'sCatherdral.jpg";


export default {
  name: "section3",
  data() {
    return {
      selectedRegion: "All" as string,
      regions: ["All", "Asia", "Europe", "America", "Oceania"] as string[],
      selectedYearRange: [1000, 1300] as number[], // 현재 지정된 연도 구간
      tempYearRange: [1000, 1300] as number[], // 연도 구간 선택 중일 경우 활용
      yearOptions: [1000, 1300, 1700, 2000] as number[],
      cards: [
        {
          id: 1,
          region: "Europe",
          img: img1,
          alt: "Italy_Pica",
          title: "Italy, Pica",
          year: 1173,
          text: `The Leaning Tower of Pisa, or simply the Tower of Pisa (torre di Pisa), is the campanile, or freestanding
            bell tower, of Pisa Cathedral. It is known for its nearly four-degree lean, the result of an
            unstable foundation. The tower is one of three structures in the Pisa's Cathedral Square (Piazza del Duomo),
            which includes the cathedral and Pisa Baptistry.`
        },
        {
          id: 2,
          region: "Europe",
          img: img2,
          alt: "Spain_Sagrada",
          title: "Spain, Sagrada Família",
          year: 1882,
          text: `The Basílica i Temple Expiatori de la Sagrada Família, otherwise known as Sagrada Família, is a church
            under construction in the Eixample district of Barcelona,  Catalonia, Spain. It is the largest unfinished Catholic church in the world. 
            Designed by Catalan architect Antoni Gaudí (1852-1926), in 2005 his work on Sagrada Família was added to an existing (1984)
            UNESCO World Heritage Site, "Works of Antoni Gaudí". On 7 November 2010, Pope Benedict XVI consecrated the church and proclaimed it a minor basilica.`
        },
        {
          id: 3,
          region: "America",
          img: img3,
          alt: "US_Fallingwater",
          title: "US, Fallingwater",
          year: 1935,
          text: `Fallingwater is a house designed by the architect Frank Lloyd Wright in 1935. Situated in the Mill Run section of Stewart township, 
            in the Laurel Highlands of southwest Pennsylvania, about 70 miles (110 km) southeast of Pittsburgh in the United States, 
            it is built partly over a waterfall on the Bear Run river. 
            The house was designed to serve as a weekend retreat for Liliane and Edgar J. Kaufmann, the owner of Pittsburgh's Kaufmann's Department Store.`
        },
        {
          id: 4,
          region: "Europe",
          img: img4,
          alt: "Russia_Basil'sCatherdral",
          title: "Russia, Saint Basil's Cathedral",
          year: 1561,
          text: `The Cathedral of Vasily the Blessed (Russian: Co6op Василия Блаженного, romanized: Sobor Vasiliya Blazhennogo), 
            known in English as Saint Basil's Cathedral, is an Orthodox church in Red Square of Moscow, and is one of the most popular cultural symbols of Russia.`
        }
      ]
    };
  },
  computed: {
    gradientStyle() {
      if (this.selectedYearRange.length < 2) {
        return { display: "none" };
      } else {
        const yearMap: Record<number, number> = {
          1000: 7,
          1300: 36,
          1700: 64,
          2000: 90
        };
        const [start, end] = this.selectedYearRange;
        const left = yearMap[start];
        const width = yearMap[end] - left;
        return {
          left: left + "%",
          width: width + "%",
        };
      }
    },
    filteredCards() {
      let filtered = this.selectedRegion === "All"
        ? this.cards
        : this.cards.filter(card => card.region === this.selectedRegion);

      if (this.selectedYearRange.length === 2) {
        const [start, end] = this.selectedYearRange;
        filtered = filtered.filter(card => card.year >= start && card.year <= end);
      } else {
        filtered = filtered.filter(card => card.year >= this.tempYearRange[0] && card.year <= this.tempYearRange[1]);
      }

      return filtered;
    },
  },
  methods: {
    selectYear(year: number) {
      if (this.selectedYearRange.length === 0) {
        this.selectedYearRange = [year];
      } else if (this.selectedYearRange.length === 1) {
        if (this.selectedYearRange[0] === year) return false;
        this.selectedYearRange.push(year);
        this.selectedYearRange.sort((a, b) => a - b);

        this.tempYearRange = [...this.selectedYearRange];
      } else {
        this.selectedYearRange = [year];
      }
    },
    isInRange(year: number) {
      if (this.selectedYearRange.length < 2) return false;
      const [start, end] = this.selectedYearRange;
      return year > start && year < end;
    }
  },
};
</script>

<style lang="css" scoped>
.contents-wrap {
  width: 1680px;
  height: 864px;
}

.subheading {
  height: 71px;
  margin: 120px 0 60px 80px;
  font-family: 'Exo 2';
  font-size: 48px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.72px;
  text-align: left;
  color: #000;
}

.filter-wrap {
  display: flex;
  margin-left: 80px;
  gap: 20px;
}

.filter-wrap ul {
  margin: 0;
  display: flex;
  list-style: none;
  width: 408px;
  height: 50px;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 5px;
  border-radius: 50px;
  border: solid 1px rgba(0, 0, 0, 0.5);
  background-color: #fff;
  cursor: pointer;
}

.region-filter {
  gap: 5px;
}

.region-filter li {
  display: flex;
  height: 40px;
  min-width: 38px;
  justify-content: center;
  align-items: center;
  padding: 5px 16px;
  font-family: 'Exo 2';
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  text-align: center;
  color: #000;
}

.region-filter li.active {
  border-radius: 25px;
  min-width: 38px;
  padding: 5px 16px;
  justify-content: center;
  background-color: #000;
  color: white;
}

.year-filter-wrap {
  position: relative;
}

.year-filter {
  position: relative;
  gap: 80px;
}

.year-filter::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 5%;
  width: 90%;
  height: 10px;
  background: #999;
  z-index: 0;
  transform: translateY(-50%);
}

.year-filter-active {
  position: absolute;
  top: 50%;
  left: 5%;
  width: 90%;
  height: 10px;
  background: #000;
  transform: translateY(-50%);
  z-index: 1;
}

.year-filter li {
  display: flex;
  position: relative;
  width: 40px;
  height: 40px;
  justify-content: center;
  align-items: center;
  font-family: 'Exo 2';
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  background: #999999;
  border-radius: 50%;
  color: #fff;
  z-index: 9;
}

.year-filter li.selected {
  background: #000;
}

.year-filter li.inRange {
  background: #000;
}

.cards-wrap {
  display: flex;
  gap: 40px;
  margin: 68px 0 0 80px;
  overflow: hidden;
  overflow-x: auto;
}

.finding-wrap {
  display: flex;
  margin: 68px 0 0 80px;
  align-items: center;
}

.no-cards-wrap {
  display: flex;
  margin-left: 80px;
  align-items: center;
}

.info-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
  font-family: Montserrat;
  width: 400px;
  height: 415px;
  padding: 15px 0 8px;
  border-radius: 10px;
  background-color: rgba(217, 217, 217, 0.5);
  margin-bottom: 24px;
}

.no-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  font-family: Montserrat;
  width: 400px;
  height: 415px;
  padding: 15px 0 8px;
  border-radius: 10px;
  background-color: rgba(217, 217, 217, 0.5);
  margin-bottom: 24px;
}

.no-card img {
  width: 200px;
  height: 200px;
  object-fit: cover;
  object-position: center;
  transform: translateY(-15%);
}

.no-card p {
  margin: -13% 0 0 0;
}

.info-card .card-header-wrap {
  display: flex;
  justify-content: space-between;
  width: 360px;
  font-size: 16px;
  margin-bottom: 9px;
}

.info-card .card-header-wrap .title {
  margin: 0;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  color: #000;
}

.info-card .card-header-wrap .year {
  margin: 0;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.24px;
  color: #000;
}

.info-card img {
  width: 360px;
  height: 227px;
  flex-grow: 0;
  object-fit: contain;
  border-radius: 5px;
}

.info-card .info-detail {
  display: -webkit-box;
  width: 360px;
  margin: 20px 0 0 3px;
  font-family: Montserrat;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: -0.21px;
  text-align: left;
  color: #000;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 6;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>