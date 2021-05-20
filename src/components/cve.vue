<template>
    <div>
      <h3> {{  }} </h3>
    </div>
</template>

<script>
/* eslint-disable max-len */
/*
does waht i need: https://github.com/foxbenjaminfox/vue-async-computed/blob/master/README.md
the api source: https://csrc.nist.gov/CSRC/media/Projects/National-Vulnerability-Database/documents/web%20service%20documentation/Automation%20Support%20for%20CVE%20Retrieval.pdf
*/
const axios = require('axios');

async function request() {
  const options = {
    method: 'get',
    // url: 'https://access.redhat.com/hydra/rest/securitydata/cvrf.json',
    url: 'https://services.nvd.nist.gov/rest/json/cves/1.0',

    headers: { 'Content-Type': 'application/json' },
  };
  const res = await axios(options);
  // console.log('data: ', res.data.result.CVE_Items[0].configurations.nodes[0].cpe_match[0].cpe23Uri);
  return res;
}

request();

export default {
  data() {
    return {
      cveName: '',
    };
  },
  created() {
    // const res = await axios(options);
  },
  mounted() {
    async function wait() {
      const dataobj = await request();
      // this.cveN = dataobj.data.result.CVE_Items[0].configurations.nodes[0].cpe_match[0].cpe23Uri;
      const temp = dataobj.data.result.CVE_Items[0].configurations.nodes[0].cpe_match[0].cpe23Uri;
      console.log('wait: ', JSON.stringify(temp));
      this.cveName = JSON.stringify(temp);
    }
    this.cveN = wait();
    // const dataobj = request();
    // this.cveN = dataobj.data.result.CVE_Items[0].configurations.nodes[0].cpe_match[0].cpe23Uri;
  },

};
</script>
