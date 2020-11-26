<template>
  <div>
    <el-container>
      <el-header><h1>199班随机选人</h1></el-header>
      <el-main class="my-main">
        <el-row :push="10" :span="4">
          要选
          <el-input-number v-model="number" :min="1" :max="maxNumber" label="选择人数"></el-input-number>
          人
        </el-row>
        <el-row :push="6" :span="12" class="my-top">
          <el-button @click="startPicking" type="primary" round>开始选人</el-button>
          <el-button @click="clearResult" type="primary" round>清空结果</el-button>
        </el-row>
        <el-col :push="6" :span="12">
          <el-card :gutter="20" class="my-top">
            <div v-for="name in selectedPeople" :key="name">
              {{name}}
            </div>
          </el-card>
        </el-col>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import Global from './Global'

export default {
  name: 'Home',
  data () {
    return {
      siteInfo: {
        copyright: '版权所有 © '
      },
      number: 1,
      maxNumber: 0,
      people: [],
      selectedPeople: [],
      isSelected: false
    }
  },
  created () {
    let date = new Date()
    this.siteInfo.copyright += date.getFullYear()
    this.people = Global.classmate.concat()
    this.maxNumber = this.people.length
  },
  methods: {
    startPicking () {
      if (this.isSelected) {
        this.$notify({
          title: '提示',
          message: '请清空上一次的结果后再选择'
        })
        return
      }
      let temp = this.number
      while (temp > 0) {
        let len = this.people.length
        let s = Math.floor(Math.random() * len)
        this.selectedPeople.push(this.people[s])
        this.people.splice(s, 1)
        temp -= 1
      }
      this.isSelected = true
    },
    clearResult () {
      this.isSelected = false
      this.people = Global.classmate.concat()
      this.maxNumber = this.people.length
      this.selectedPeople.splice(0, this.selectedPeople.length)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .my-top {
    margin-top: 25px;
  }
  .my-main {
    min-height: calc(100vh - 70px - 60px);
  }
  .my-footer {
    width: 100%;
    height: 50px;
  }
  .copyright {
    margin: 10px;
    color: #333;
  }
  .copyright>a {
    color: #777;
  }
</style>
