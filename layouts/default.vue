<template>
  <v-app light>
    <div class="nav-bar d-flex flex-row justify-space-between">
      <div class="d-flex flex-row">
        <div class="">
          <img height="60px" :src="logo_bkk" alt="" />
        </div>
        <div class="d-flex flex-column ml-4">
          <div class="primary--text title-web">ข้อมูลการจัดซื้อจัดจ้างกรุงเทพมหานคร</div>
          <div>Bangkok Procurement</div>
        </div>
      </div>
      <div class="d-flex flex-row">
        <v-btn color="#17985A" depressed dark>เข้าสู่ระบบ</v-btn>
        <div class="mx-2">
          <v-text-field label="ค้นหา" outlined dense></v-text-field>
        </div>
        <div class="mx-1"><v-btn outlined> ก </v-btn></div>
        <div class="mx-1"><v-btn outlined> TH </v-btn></div>
      </div>
    </div>
    <div class="menu-bar">
      <div>ภาพรวม</div>
      <div>รายการจัดซื้อจัดจ้าง</div>
      <div>ความคืบหน้าของโครงการ</div>
      <div>GOV</div>
      <div>รายการประมูล</div>
    </div>
    <Nuxt />
    <v-divider color="primary"></v-divider>
    <v-footer color="white" class="container">
      <v-row class="py-6">
        <v-col cols="6" class="d-flex flex-row">
          <div><img :src="logo_bkk" alt="" /></div>
          <div class="pl-6">
            <h2 class="primary--text font-weight-normal">กรุงเทพมหานคร</h2>
            <p>
              173 ศาลาว่าการกรุงเทพมหานคร (เสาชิงช้า) ถนนดินสอ<br />แขวงเสาชิงช้า
              เขตพระนคร กรุงเทพมหานคร 10200
            </p>
          </div>
        </v-col>
        <v-col cols="4">
          <div class="d-flex flex-row align-center mb-4">
            <div>
              <img width="30px" :src="footer_tel" alt="" />
            </div>
            <div class="pl-6">
              <div>โทรศัพท์กลาง</div>
              <div class="primary--text">0-2221-2141-69</div>
            </div>
          </div>
          <div class="d-flex flex-row align-center my-4">
            <div>
              <img width="30px" :src="footer_email" alt="" />
            </div>
            <div class="pl-6">
              <div>อีเมล</div>
              <div class="primary--text">contact@bangkok.go.th</div>
            </div>
          </div>
        </v-col>
        <v-col cols="2">
          <div class="primary--text font-weight-bold">แผนผังเว็บไซต์</div>
        </v-col>
      </v-row>
      <div class="d-flex flex-row justify-center footer-copyright-area">
        <div class="footer-copyright">
          © Copyright By
          โครงการพัฒนาระบบเผยแพร่ข้อมูลการจัดซื้อจัดจ้างตามพระราชบัญญัติฉบับใหม่ (E-GP)
          All Rights Reserved.
        </div>
      </div>
    </v-footer>
  </v-app>
</template>

<script>
import logo_bkk from "@/assets/logo-bkk.svg";
import footer_tel from "@/assets/tel.svg";
import footer_email from "@/assets/email.svg";
export default {
  name: "DefaultLayout",
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      logo_bkk: logo_bkk,
      footer_tel: footer_tel,
      footer_email: footer_email,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
      map: undefined,
    };
  },
  mounted() {
    const mapboxgl = require("mapbox-gl");
    this.map = new mapboxgl.Map({
      accessToken:
        "pk.eyJ1Ijoibml0aWtvcm4iLCJhIjoiY2p6ZHR0Yjk0MDNxNDNncGhqbDk5M3ZpaCJ9.FW231UaLDWmlgt3d7HQ1yg",
      container: "map", // <div id="map"></div>
      style: "mapbox://styles/mapbox/streets-v11", // default style
      center: [100.505208, 13.75842], // starting position as [lng, lat]
      zoom: 10,
    });
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Kanit:400,500,700,900");
html,
body,
#app {
  height: 100%;
  color: #052d46;
  font-family: "Kanit", Helvetica, Arial, sans-serif !important;
}
.nav-bar {
  height: 102px;
  background-color: white;
  padding: 0px 40px;
  align-items: center;
}

.menu-bar {
  background-color: #9caae4;
  height: 76px;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0px 60px;
}

.menu-bar > div {
  color: white;
  padding: 0px 20px;
}

.title-web {
  font-weight: 400;
  font-size: 22px;
}

.footer-copyright {
  color: #c5c5c5;
  font-size: 12px;
}

.footer-copyright-area {
  width: 100%;
}

.overlay-menu {
  position: absolute;
  top: 180px;
  z-index: 100000;
  width: calc(100% - 40px);
  margin: 20px;
  height: 80px;
}
</style>
