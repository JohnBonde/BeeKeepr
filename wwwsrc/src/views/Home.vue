<template>
  <div class="home container-fluid">
    <div class="row">
      <div class="col-10 home-title">
        <h1>Public Honeycombs</h1>
      </div>
      <div class="col-2 create-comb">
        <img @click="showModal" src="../assets/honeycomb.png" style="width:50px;height:50px" />
        <i class="fas fa-plus"></i>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <div class="grid">
          <ul id="hexGrid">
            <comb :combData="comb" v-for="comb in combs" :key="comb.id" />
          </ul>
        </div>
      </div>
    </div>
    <comb-modal v-show="isModalVisible" @close="closeModal" />
  </div>
</template>

<script>
import CombModal from "../components/CombModal";
import Comb from "../components/Comb";
export default {
  name: "home",
  components: {
    CombModal,
    Comb
  },
  data() {
    return { isModalVisible: false };
  },
  mounted() {
    this.$store.dispatch("getPublicKeeps");
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    combs() {
      return this.$store.state.publicKeeps;
    }
  },
  methods: {
    logout() {
      this.$store.dispatch("logout");
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  }
};
</script>
<style>
body {
  background-color: gold;
}
.create-comb {
  display: flex;
  justify-content: flex-end;
  padding: 1em;
}
.create-comb img,
.fa-plus {
  cursor: pointer;
}
.home-title h1 {
  margin-top: 18px;
}
/* Hexagons */
#hexGrid {
  display: flex;
  flex-wrap: wrap;
  width: 90%;
  margin: 0 auto;
  overflow: hidden;
  font-family: "Raleway", sans-serif;
  font-size: 15px;
  list-style-type: none;
}

.hex {
  position: relative;
  visibility: hidden;
  outline: 1px solid transparent; /* fix for jagged edges in FF on hover transition */
}
.hex::after {
  content: "";
  display: block;
  padding-bottom: 86.602%; /* =  100 / tan(60) * 1.5 */
}
.hexIn {
  position: absolute;
  width: 96%;
  padding-bottom: 110.851%; /* =  width / sin(60) */
  margin: 0 2%;
  overflow: hidden;
  visibility: hidden;
  outline: 1px solid transparent; /* fix for jagged edges in FF on hover transition */
  -webkit-transform: rotate3d(0, 0, 1, -60deg) skewY(30deg);
  -ms-transform: rotate3d(0, 0, 1, -60deg) skewY(30deg);
  transform: rotate3d(0, 0, 1, -60deg) skewY(30deg);
}
.hexIn * {
  position: absolute;
  visibility: visible;
  outline: 1px solid transparent; /* fix for jagged edges in FF on hover transition */
}
.hexLink {
  display: block;
  width: 100%;
  height: 100%;
  text-align: center;
  color: #fff;
  overflow: hidden;
  -webkit-transform: skewY(-30deg) rotate3d(0, 0, 1, 60deg);
  -ms-transform: skewY(-30deg) rotate3d(0, 0, 1, 60deg);
  transform: skewY(-30deg) rotate3d(0, 0, 1, 60deg);
  background-color: darkgoldenrod;
}

/*** HEX CONTENT **********************************************************************/

.hex h6,
.hex p {
  width: 100%;
  padding: 5%;
  box-sizing: border-box;
  background-color: rgba(110, 110, 110, 0.8);
  font-weight: 300;
  -webkit-transition: -webkit-transform 0.2s ease-out, opacity 0.3s ease-out;
  transition: transform 0.2s ease-out, opacity 0.3s ease-out;
}
.hex h6 {
  bottom: 50%;
  padding-top: 50%;
  font-size: 1.5em;
  z-index: 1;
  -webkit-transform: translate3d(0, -100%, 0);
  -ms-transform: translate3d(0, -100%, 0);
  transform: translate3d(0, -100%, 0);
}
.hex h6::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 45%;
  width: 10%;
  text-align: center;
  border-bottom: 1px solid #fff;
}
.hex p {
  top: 50%;
  padding-bottom: 50%;
  -webkit-transform: translate3d(0, 100%, 0);
  -ms-transform: translate3d(0, 100%, 0);
  transform: translate3d(0, 100%, 0);
}

/*** HOVER EFFECT  **********************************************************************/
.hexLink:hover h6,
.hexLink:focus h6,
.hexLink:hover p,
.hexLink:focus p {
  -webkit-transform: translate3d(0, 0, 0);
  -ms-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}

/*** HEXAGON SIZING AND EVEN ROW INDENTATION *****************************************************************/
@media (min-width: 1201px) {
  /* <- 5-4  hexagons per row */
  #hexGrid {
    padding-bottom: 4.4%;
  }
  .hex {
    width: 20%; /* = 100 / 5 */
  }
  .hex:nth-child(9n + 6) {
    /* first hexagon of even rows */
    margin-left: 10%; /* = width of .hex / 2  to indent even rows */
  }
}

@media (max-width: 1200px) and (min-width: 901px) {
  /* <- 4-3  hexagons per row */
  #hexGrid {
    padding-bottom: 5.5%;
  }
  .hex {
    width: 25%; /* = 100 / 4 */
  }
  .hex:nth-child(7n + 5) {
    /* first hexagon of even rows */
    margin-left: 12.5%; /* = width of .hex / 2  to indent even rows */
  }
}

@media (max-width: 900px) and (min-width: 601px) {
  /* <- 3-2  hexagons per row */
  #hexGrid {
    padding-bottom: 7.4%;
  }
  .hex {
    width: 33.333%; /* = 100 / 3 */
  }
  .hex:nth-child(5n + 4) {
    /* first hexagon of even rows */
    margin-left: 16.666%; /* = width of .hex / 2  to indent even rows */
  }
}

@media (max-width: 600px) {
  /* <- 2-1  hexagons per row */
  #hexGrid {
    padding-bottom: 11.2%;
  }
  .hex {
    width: 50%; /* = 100 / 3 */
  }
  .hex:nth-child(3n + 3) {
    /* first hexagon of even rows */
    margin-left: 25%; /* = width of .hex / 2  to indent even rows */
  }
}

@media (max-width: 400px) {
  #hexGrid {
    font-size: 13px;
  }
}
</style>
