<!-- eslint-disable max-len -->
<template>
  <q-page class="flex flex-center">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          IP-tracker
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
        <div class="q-pa-md">
          <q-btn class="fit" color="green" label="Account Settings" icon="settings">
            <q-menu>
              <div class="row no-wrap q-pa-md">
                <div class="column">
                  <div class="text-h6 q-mb-md">Settings</div>
                  <div>
                    <q-input filled v-model="inputkey" placeholder="api key" :dense="dense" :rules="[val => !!val || 'Field is required']"/>
                  </div>
                  <div class="q-pa-md">
                    <q-btn :disable="!inputkey" label="set API key" push @click="setapikey()"/>
                  </div>

                </div>

                <q-separator vertical inset class="q-mx-lg" />

                <div class="column items-center">
                  <q-avatar size="72px">
                    <img src='https://cdn.pixabay.com/photo/2020/02/15/14/33/network-4851120_960_720.jpg'>
                  </q-avatar>
                  <q-separator vertical inset class="q-mx-lg" />
                  <q-btn
                    v-if="keychek"
                    color="primary"
                    label="Logout"
                    push
                    size="sm"
                    v-close-popup
                    @click="logout()"
                  />
                </div>
              </div>
            </q-menu>
          </q-btn>

          <q-card class="q-ma-md fixed-bottom">
            <q-card-section>
              <p>You can use this App by registering for an free api key at</p>
              <a href="https://ipstack.com/product">ipstack</a>
              <br>
              <p>version 1.0.0</p>
            </q-card-section>
          </q-card>
        </div>

    </q-drawer>

    <div row >
      <div>
        <cveNews/>
      </div>
      <div class="q-pa-md">
        <h1>IP-Locator</h1>
      </div>
      <div class="q-pa-md row no-wrap items-center ">
        <div id="scene" ref="sc">
          <p>if u can see this the gloge.gl isn't working!</p>
        </div>
        <!-- threeJS :lat="latitude" :lon="longitude"/-->
      </div>
      <div class="q-pa-md row no-wrap items-center ">
        <div class="q-pa-md">
          <q-input filled v-model="input" placeholder="ip/host" :dense="dense" :rules="[val => !!val || 'Field is required']"/>
        </div>
        <div class="q-pa-md">
          <q-btn :disable="!input" label="Go" icon="search" @click="init()"></q-btn>
        </div>
      </div>
      <div class="q-pa-md row no-wrap items-center">
        <div class="q-pa-md" >
          <q-markup-table flat bordered dark>
            <thead class="bg-teal">
              <tr>
                <th colspan="7">
                  <div class="row no-wrap items-center">
                    <q-img
                      style="width: 90px"
                      :ratio="1"
                      class="rounded-borders"
                      :src= 'imgsrc'
                    />
                    <div class="text-h4 q-ml-md text-white">IP-Data</div>
                    <div class="text-h5 q-ml-md text-white">{{hostname}}</div>
                  </div>
                </th>
              </tr>
            </thead>
            <tbody >
              <tr>
                <th class="text-left">IP</th>
                <th class="text-right">type (ipv4/ipv6)</th>
                <th class="text-right">Continent</th>
                <th class="text-right">Country name</th>
                <th class="text-right">Region code</th>
                <th class="text-right">City</th>
                <th class="text-right">Zip</th>
              </tr>
              <tr>
                <td class="text-left">{{host}}</td>
                <td class="text-middle">{{type}}</td>
                <td class="text-middle">{{continent}}</td>
                <td class="text-middle">{{country}}</td>
                <td class="text-middle">{{regionC}}</td>
                <td class="text-middle">{{city}}</td>
                <td class="text-middle">{{zip}}</td>
              </tr>
            </tbody>
          </q-markup-table>
        </div>
        <div class="q-pa-md">
          <q-btn icon="download" label="download" @click="download()"></q-btn>
        </div>
      </div>
    </div>

  </q-page>
</template>

<script>
/* eslint-disable vue/no-unused-components */
/* eslint-disable no-unused-vars */
/* eslint-disable max-len */
import { Notify } from 'quasar';
import Globe from 'globe.gl';

const ipstack = require('ipstack');
const FileSaver = require('file-saver');

const myglobe = Globe();

const gdata = [{
  lat: 0,
  lng: 0,
  size: 0.5,
  color: '#fc0303',
  name: '',
}];

let container = {};

function renderGlobe(con, dat) {
  myglobe.width(container.offsetWidth);
  myglobe.height(container.offsetHeight);
  myglobe.globeImageUrl('https://unpkg.com/three-globe/example/img/earth-night.jpg');
  myglobe(con);
  myglobe(con).pointsData(dat).pointColor('color').pointLabel('name')
    .pointAltitude(0)
    .pointRadius(0.75);
  /* myglobe(con).labelsData(dat).labelLat('lat').labelLng('lon')
    .labelText('text')
    .labelSize('size')
    .labelDotRadius('size')
    .labelResolution(2); */
}

export default {
  name: 'PageIndex',

  data() {
    return {
      toggle1: true,
      toggle2: false,
      leftDrawerOpen: false,
      hostname: '',
      host: '',
      input: '',
      inputkey: '',
      dense: false,
      imgsrc: 'https://cdn.pixabay.com/photo/2020/02/15/14/33/network-4851120_960_720.jpg',
      type: '',
      continent: '',
      country: '',
      regionC: '',
      city: '',
      zip: '',
      keychek: false,
      dataobj: { },
      latitude: 'test',
      longitude: '',
    };
  },
  computed: {

  },
  components: {
    // eslint-disable-next-line global-require
    // threeJS: require('components/three.vue').default,
    // eslint-disable-next-line global-require
    cveNews: require('components/cve.vue').default,
  },

  mounted() {
    container = this.$refs.sc;
    // renderGlobe(container, gdata);
  },

  methods: {
    setapikey() {
      const api = this.inputkey;
      localStorage.setItem('key', api);
      this.inputkey = '';
      this.keychek = true;
    },
    logout() {
      localStorage.clear();
      this.keychek = false;
    },

    init() {
      function setData(data) {
        const ipdata = {
          host: data.ip,
          type: data.type,
          regionN: data.region_name,
          flag: data.location.country_flag,
          continent: data.continent_name,
          country: data.country_name,
          regionC: data.region_code,
          city: data.city,
          zip: data.zip,
          lat: data.latitude,
          lon: data.longitude,
        };
        return ipdata;
      }

      const ip = this.input;
      this.input = '';
      this.hostname = ip;

      if (localStorage.getItem('key')) {
        const token = localStorage.getItem('key');

        ipstack(ip, token, (err, res) => {
          // location = res;
          try {
            this.dataobj = setData(res);
            // setData(res, location);
            this.host = this.dataobj.host;
            this.type = this.dataobj.type;
            this.bundesland = this.dataobj.regionN;
            this.regionC = this.dataobj.regionC;
            this.imgsrc = this.dataobj.flag;
            this.continent = this.dataobj.continent;
            this.country = this.dataobj.country;
            this.city = this.dataobj.city;
            this.zip = this.dataobj.zip;
            this.latitude = parseInt(this.dataobj.lat, 10);
            this.longitude = parseInt(this.dataobj.lon, 10);
          } catch (error) {
            Notify.create({ type: 'warning', message: error.toString(), position: 'center' });
          }
          container = this.$refs.sc;
          gdata[0].lat = this.latitude;
          gdata[0].lng = this.longitude;
          gdata[0].name = this.city;
          myglobe.pointsData(gdata);
        });
      } else {
        Notify.create({ type: 'negative', message: 'No Api key set', position: 'center' });
      }
    },
    download() {
      const data = JSON.stringify(this.dataobj);
      const blob = new Blob([data], { type: 'application/json' });
      FileSaver.saveAs(blob, `${this.hostname}.json`);
    },
  },

};

</script>
