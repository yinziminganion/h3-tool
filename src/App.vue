<template>
  <div>
    <el-form :model="form" label-width="150px">
      <el-form-item label="h3 id">
        <el-input v-model="form.h3Id" style="width: 200px" />
      </el-form-item>
      <el-form-item label="操作">
        <div class="buttons">
          <el-button type="primary" @click="geoToH3">获取坐标所在H3↑</el-button>
          <el-button type="primary" @click="h3ToGeo"> 获取H3中心点↓</el-button>
        </div>
      </el-form-item>
      <el-form-item label="经度/longtitude">
        <el-input v-model="form.lon" style="width: 200px" />
      </el-form-item>
      <el-form-item label="纬度/latitude">
        <el-input v-model="form.lat" style="width: 200px" />
      </el-form-item>
      <el-form-item label="精度/res">
        <el-input v-model="form.res" style="width: 200px" />
      </el-form-item>
      <el-form-item label="边界">
        <el-input v-model="form.h3Bound" :autosize="{ minRows: 1, maxRows: 32 }" type="textarea" readonly />
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
const h3 = require('h3-js')
const bigInt = require('big-integer')

export default {
  name: 'App',
  data() {
    return {
      form: {
        h3Id: '622370470627672063',
        lon: '',
        lat: '',
        res: 10,
        h3Bound: [],
      },
    }
  },
  methods: {
    h3ToGeo() {
      console.log('begin h3ToGeo', this.form)
      const n = bigInt(this.form.h3Id)
      console.log('n', n)
      let geo = h3.h3ToGeo(n.toString(16))
      console.log('n16', n.toString(16))
      console.log('geo', geo)
      this.form.lon = geo[1]
      this.form.lat = geo[0]
      const bound = h3.h3ToGeoBoundary(n.toString(16)).map(item => [item[1], item[0]])
      this.form.h3Bound = JSON.stringify(bound, null, 4)
      console.log('end h3ToGeo', this.form)
    },
    geoToH3() {
      console.log('begin h3ToGeo', this.form)
      const n16 = bigInt(h3.geoToH3(this.form.lat, this.form.lon, this.form.res), 16)
      this.form.h3Id = n16.toString(10)
      console.log('end h3ToGeo', this.form)
    },
  }
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
