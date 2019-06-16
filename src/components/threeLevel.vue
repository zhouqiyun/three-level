<template>
  <div class="three-level">
    <el-select
      v-model="sheng"
      @change="selectProvince"
      placeholder="省级地区">
      <el-option
        v-for="item in province"
        :key="item.id"
        :label="item.value"
        :value="item.value">
      </el-option>
    </el-select>
    <el-select
      v-model="shi"
      @change="selectCity"
      placeholder="市级地区">
      <el-option
        v-for="item in shi1"
        :key="item.id"
        :label="item.value"
        :value="item.value">
      </el-option>
    </el-select>
    <el-select
      v-model="qu"
      @change="selectBlock"
      placeholder="区级地区">
      <el-option
        v-for="item in qu1"
        :key="item.id"
        :label="item.value"
        :value="item.value">
      </el-option>
    </el-select>
  </div>
</template>

<script>
import mapJson from '../utils/map.json'
export default {
  data () {
    return {
      province: [],
      city: [],
      block: [],
      sheng: '',
      shi: '',
      shi1: [],
      qu: '',
      qu1: []
    }
  },
  methods: {
    getCityData: function () {
      let data = mapJson
      // 省市区数据分类
      for (let item in data) {
        if (item.match(/0000$/)) {
          this.province.push({ id: item, value: data[item], children: [] })
        } else if (item.match(/00$/)) {
          this.city.push({ id: item, value: data[item], children: [] })
        } else {
          this.block.push({ id: item, value: data[item] })
        }
      }
      // 分类市级
      for (let index in this.province) {
        for (let index1 in this.city) {
          if (this.province[index].id.slice(0, 2) === this.city[index1].id.slice(0, 2)) {
            this.province[index].children.push(this.city[index1])
          }
        }
      }
      // 分类区级
      for (var item1 in this.city) {
        for (var item2 in this.block) {
          if (this.block[item2].id.slice(0, 4) === this.city[item1].id.slice(0, 4)) {
            this.city[item1].children.push(this.block[item2])
          }
        }
      }
    },
    // 选省
    selectProvince: function (e) {
      for (var index2 in this.province) {
        if (e === this.province[index2].value) {
          this.shi1 = this.province[index2].children
          this.shi = ''
          // this.shi = this.province[index2].children[0].value
          // this.qu1 = this.province[index2].children[0].children
          this.qu = ''
          // this.qu = this.province[index2].children[0].children[0].value
          // this.E = this.qu1[0].id
        }
      }
    },
    // 选市
    selectCity: function (e) {
      for (var index3 in this.city) {
        if (e === this.city[index3].value) {
          this.qu1 = this.city[index3].children
          this.qu = ''
          // this.qu = this.city[index3].children[0].value
          // this.E = this.qu1[0].id
        }
      }
    },
    // 选区
    selectBlock: function (e) {
      // this.E = e
    }
  },
  created: function () {
    this.getCityData()
  }
}
</script>

<style lang="less" scoped>

</style>
