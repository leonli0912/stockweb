<template>
    <v-card
    class="mx-auto"
  >
  <v-container class="grey lighten-5">
    <v-layout
      text-center
      wrap
    >
    <v-row no-gutters>      
        
    </v-row>   
    </v-layout>

    <v-row
      class="mb-6"
      no-gutters
    >
    <v-col sm="9">
      <h1 class="table_title">
        今年股息率最高排名
      </h1>
 
    </v-col>
    <v-col sm="3">
      <v-autocomplete
        v-model="model"
        :items="stockCodes"
        :loading="isLoading"
        :search-input.sync="search"
        color="white"
        hide-no-data
        hide-selected
        item-text="Description"
        item-value="API"
        label="StockCodes"
        placeholder="Start typing to Search"
        prepend-icon="mdi-database-search"
        return-object
      >        
      </v-autocomplete>
    </v-col>
      <!--<v-col
      sm="4"      
      >
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
      </v-col>-->
    </v-row>
    
    <v-row no-gutters>
      <v-flex xs12>
        <v-data-table
        :headers="headers"
        :items="stocks"
        :items-per-page="10"
        class="elevation-1"
        >          
        </v-data-table>
      <p >
        For help and collaboration please join our online
        <a href="http://stocktest.hronyun.cn" target="_blank">Stock Community</a>
      </p> 
      </v-flex>
    </v-row>
  </v-container>

</v-card>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'

export default {
  data: () => ({
    headers: [
          {
            text: 'StockCode',
            align: 'left',
            sortable: false,
            value: 'stockId',
          },
          { text: 'Year', value: 'reportYear' },
          { text: 'Dividend Date', value: 'dividendDate' },
          { text: 'Description', value: 'divdendContent' },
          { text: 'Ratio', value: 'ratio' },
    ],
    stocks:[],
    stockCodes:['sh603328','sh600126'],
    desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
    ],
    ecosystem: [
      {
        text: 'vuetify-loader',
        href: 'https://github.com/vuetifyjs/vuetify-loader',
      },
      {
        text: 'github',
        href: 'https://github.com/vuetifyjs/vuetify',
      },
      {
        text: 'awesome-vuetify',
        href: 'https://github.com/vuetifyjs/awesome-vuetify',
      },
    ],
    importantLinks: [
      {
        text: 'Documentation',
        href: 'https://vuetifyjs.com',
      },
      {
        text: 'Chat',
        href: 'https://community.vuetifyjs.com',
      },
      {
        text: 'Made with Vuetify',
        href: 'https://madewithvuejs.com/vuetify',
      },
      {
        text: 'Twitter',
        href: 'https://twitter.com/vuetifyjs',
      },
      {
        text: 'Articles',
        href: 'https://medium.com/vuetify',
      },
    ],
    whatsNext: [
      {
        text: 'Explore components',
        href: 'https://vuetifyjs.com/components/api-explorer',
      },
      {
        text: 'Select a layout',
        href: 'https://vuetifyjs.com/layout/pre-defined',
      },
      {
        text: 'Frequently Asked Questions',
        href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
      },
    ],
  }),
  mounted(){
    const APIURL = 'http://stocktest.ap-northeast-2.elasticbeanstalk.com/allDividend?year=2019';
    axios.get(APIURL).then(response => {
      response.data.forEach(item =>{
        console.log(item);
        item.dividendDate = timestampToTime(item.dividendDate);
        this.stocks.push(item)

      })
    })
    console.log(this.stocks)
  }
};

function timestampToTime(timestamp) {
     let date = new Date(timestamp);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
     let Y = date.getFullYear() + '-';
     let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
     let D = date.getDate() + ' ';
     let h = date.getHours() + ':';
     let m = date.getMinutes() + ':';
     let s = date.getSeconds();
     return Y+M+D;
}

</script>
<style type="text/css">
.table_title {
    float: left;
    font-weight: normal;
    margin-right: 12px;
    max-width: 18em;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
</style>