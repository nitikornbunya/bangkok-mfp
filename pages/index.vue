<template>
  <v-col class="pa-0" justify="center" align="center">
    <div class="overlay-menu">
      <v-card class="pa-2">
        <div class="d-flex flex-row justify-space-between">
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="สังกัด"></v-select>
          </div>
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="เขต"></v-select>
          </div>
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="ความเสี่ยงทุจริต"></v-select>
          </div>
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="สถานะโครงการ"></v-select>
          </div>
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="มูลค่าโครงการ"></v-select>
          </div>
          <div class="px-4">
            <v-select :items="organization" v-model="select" label="ปีงบประมาณ"></v-select>
          </div>
        </div>
      </v-card>
    </div>
    <div id="map" style="width: 100%; height: 450px"></div>
    <div class="search-area d-flex flex-column">
      <h1>ระบุคำค้นหา</h1>
      <div class="search-row d-flex flex-row mt-6 justify-center">
        <div>
          <v-text-field
            label="ค้นหาโครงการ"
            placeholder="ค้นหาโครงการ"
            outlined
            dense
          ></v-text-field>
        </div>
        <div>
          <v-autocomplete placeholder="กลุ่มหน่วยงาน" dense solo></v-autocomplete>
        </div>
        <div>
          <v-autocomplete placeholder="ประเภทการจัดหา" dense solo></v-autocomplete>
        </div>
        <div>
          <v-btn color="#9CAAE4" dark>ค้นหา</v-btn>
        </div>
        <div>
          <v-btn color="#9CAAE4" outlined>ค้นหาแบบละเอียด</v-btn>
        </div>
      </div>
    </div>
    <div class="corrupt-area container">
      <div class="d-flex flex-column justify-start my-8">
        <div class="title-project text-left">
          <b> โครงการที่มีแนวโน้มการทุจริตสูงสุด</b>
        </div>
        <div class="subtitle-project text-left">
          5 อันดับแรกของโครงการที่มีคะแนนแนวโน้มการทุจริตสูงสุด
        </div>
      </div>
      <div>
        <v-data-table
          :headers="corrupt_headers"
          :items="corrupt_item"
          class="elevation-0"
          :hide-default-footer="true"
        >
          <template v-slot:item.name="{ item }">
            <div class="py-4">
              <div class="secondary--text">
                {{ item.name }}
                <b class="black--text">{{ item.organization }}</b>
              </div>
              <div class="description-table">{{ item.description }}</div>
            </div>
          </template>
          <template v-slot:item.budget="{ item }">
            <div>
              <b>
                {{ item.budget }}
              </b>
            </div>
          </template>
          <template v-slot:item.risk="{ item }">
            <div class="red--text">
              {{ item.risk }}
            </div>
          </template>
        </v-data-table>
      </div>
    </div>
    <v-divider class="container"></v-divider>
    <div class="corrupt-area container">
      <div class="d-flex flex-column justify-start my-8">
        <div class="title-project text-left">
          <b>โครงการใหม่ในกรุงเทพมหานคร</b>
        </div>
        <div class="subtitle-project text-left">รายละเอียดโครงการใหม่ในกรุงเทพมหานคร</div>
      </div>
      <div>
        <v-data-table
          :headers="corrupt_headers"
          :items="corrupt_item"
          class="elevation-0"
          :hide-default-footer="true"
        >
          <template v-slot:item.name="{ item }">
            <div class="py-4">
              <div class="secondary--text">
                {{ item.name }}
                <b class="black--text">{{ item.organization }}</b>
              </div>
              <div class="description-table">{{ item.description }}</div>
            </div>
          </template>
          <template v-slot:item.budget="{ item }">
            <div>
              <b>
                {{ item.budget }}
              </b>
            </div>
          </template>
          <template v-slot:item.risk="{ item }">
            <div>
              <img width="40px" :src="img_document" alt="">
            </div>
          </template>
        </v-data-table>
      </div>
    </div>
    <v-divider class="container"></v-divider>
    <div class="container">
      <h1 class="my-8">หมวดหมู่การจัดซื้อจัดจ้าง</h1>

      <v-row class="align-end my-10" no-gutters>
        <v-col v-for="item in procurement_1" :key="item.name">
          <div class="d-flex flex-column justify-center align-center">
            <img :src="item.img" class="pa-4" width="100px" alt="" />
            {{ item.name }}
            <div class="procurement-more secondary--text">ดูรายการจัดซื้อ ></div>
          </div>
        </v-col>
      </v-row>
      <v-divider></v-divider>
      <v-row class="align-end my-10" no-gutters>
        <v-col v-for="item in procurement_2" :key="item.name">
          <div class="d-flex flex-column justify-center align-center">
            <img :src="item.img" class="pa-4" width="100px" alt="" />
            {{ item.name }}
            <div class="procurement-more secondary--text">ดูรายการจัดซื้อ ></div>
          </div>
        </v-col>
      </v-row>
    </div>
  </v-col>
</template>

<script>
import img_icon1 from "@/assets/procurement/icon1.svg";
import img_document from "@/assets/document.svg";
const mapboxgl = require("mapbox-gl");

export default {
  name: "IndexPage",
  data: () => ({
    corrupt_headers: [
      {
        text: "ชื่อโครงการ",
        align: "left",
        value: "name",
        width: "50%",
        sortable: false,
      },
      { text: "วงเงินงบประมาณ", align: "right", value: "budget", width: "20%" },
      {
        text: "จำนวนผู้ประมูล",
        align: "right",
        value: "company",
        width: "15%",
      },
      { text: "ความเสี่ยง", align: "right", value: "risk", width: "15%" },
    ],
    corrupt_headers2: [
      {
        text: "ชื่อโครงการ",
        align: "left",
        value: "name",
        width: "50%",
        sortable: false,
      },
      { text: "วงเงินงบประมาณ", align: "right", value: "budget", width: "20%" },
      {
        text: "จำนวนผู้ประมูล",
        align: "right",
        value: "company",
        width: "15%",
      },
      { text: "เอกสาร", align: "center", value: "risk", width: "15%" },
    ],
    corrupt_item: [
      {
        name: "64097154557",
        organization: "สำนักงานเขต A",
        description:
          "จ้างปรับปรุงอาคารบริการชั้น 3 เป็นหอพักผู้ป่วยวิกฤตทางเดินหายใจแรงดันลบ โดยวิธีคัดเลือก",
        budget: "21,122,000.00 บาท",
        company: 1,
        risk: 99,
      },
      {
        name: "63117397510",
        organization: "สำนักงานเขต B",
        description:
          "จ้างปรับปรุงอาคารบริการชั้น 3 เป็นหอพักผู้ป่วยวิกฤตทางเดินหายใจแรงดันลบ โดยวิธีคัดเลือก",
        budget: "2,239,000.00 บาท",
        company: 1,
        risk: 84.5,
      },
      {
        name: "63107084951",
        organization: "สำนักงานเขต C",
        description:
          "จ้างปรับปรุงอาคารบริการชั้น 3 เป็นหอพักผู้ป่วยวิกฤตทางเดินหายใจแรงดันลบ โดยวิธีคัดเลือก",
        budget: "1,057,000.00 บาท",
        company: 1,
        risk: 81.1,
      },
      {
        name: "63117109356",
        organization: "สำนักงานเขต D",
        description:
          "จ้างปรับปรุงอาคารบริการชั้น 3 เป็นหอพักผู้ป่วยวิกฤตทางเดินหายใจแรงดันลบ โดยวิธีคัดเลือก",
        budget: "360,000.00 บาท",
        company: 1,
        risk: 79.5,
      },
      {
        name: "64097154557",
        organization: "สำนักงานเขต E",
        description:
          "จ้างปรับปรุงอาคารบริการชั้น 3 เป็นหอพักผู้ป่วยวิกฤตทางเดินหายใจแรงดันลบ โดยวิธีคัดเลือก",
        budget: "21,122,000.00 บาท",
        company: 1,
        risk: 77.2,
      },
    ],
    procurement_1: [
      { name: "วัสดุครุภัณฑ์", img: require("@/assets/procurement/icon1.svg") },
      {
        name: "ที่ดินและสิ่งก่อสร้าง",
        img: require("@/assets/procurement/icon2.svg"),
      },
      { name: "จ้าง/รับเหมา", img: require("@/assets/procurement/icon3.svg") },
      { name: "จ้างควบคุมงาน", img: require("@/assets/procurement/icon4.svg") },
    ],
    procurement_2: [
      {
        name: "จ้างทำของ/จ้างเหมาบริการ",
        img: require("@/assets/procurement/icon5.svg"),
      },
      { name: "เช่า", img: require("@/assets/procurement/icon6.svg") },
      { name: "จ้างที่ปรึกษา", img: require("@/assets/procurement/icon7.svg") },
      { name: "จ้างออกแบบ", img: require("@/assets/procurement/icon8.svg") },
    ],
    img_icon1: img_icon1,
    organization: ['ทั้งหมด', 'สำนักการจราจรและขนส่ง'],
    select: "ทั้งหมด",
    img_document: img_document
  }),
  mounted() {
    this.map = new mapboxgl.Map({
      accessToken:
        "pk.eyJ1Ijoibml0aWtvcm4iLCJhIjoiY2p6ZHR0Yjk0MDNxNDNncGhqbDk5M3ZpaCJ9.FW231UaLDWmlgt3d7HQ1yg",
      container: "map", // <div id="map"></div>
      style: "mapbox://styles/mapbox/streets-v11", // default style
      center: [100.529231, 13.748025], // starting position as [lng, lat]
      zoom: 10,
    });
  },
};
</script>

<style lang="scss">
.search-area {
  background-color: #fbfbfb;
  padding: 24px;
}
.search-row > div {
  margin: 0px 4px;
}
.title-project {
  font-size: 34px;
}
.subtitle-project {
  font-size: 16px;
  color: #292929;
}
.description-table {
  color: #818181;
}
.procurement-more {
  font-size: 12px;
}
</style>
