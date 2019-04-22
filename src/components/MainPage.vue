<template>
  <div class="mainpage">
    <!-- 顶部导航栏 -->
    <nav id="top-nav">
      <div class="logo">
        <img src="../assets/levelsiconfilled.png" alt="logo" />
        <span>levels</span>
      </div>
      <div class="sponsor">
        <img src="../assets/sponsormobile.png" alt="sopnsor" />
      </div>

      <el-select
        v-model="selectedJobType"
        placeholder="Select Job"
        type="round"
        class="jobTypeSelect"
      >
        <el-option
          v-for="item in jobOptions"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
    </nav>

    <!-- 主要内容 -- 表格展示职级和工资 -->
    <el-row id="main-content">
      <!-- 表格前后留白 -->
      <el-col :span="2"><div class="grid-content"></div></el-col>

      <el-col :span="20">
        <!-- 第一行是选择公司的一排按钮 -->
        <el-row class="choice-buttons">
          <el-col :span="16" class="company-buttons">
            <!-- <p>{{ jobType }}</p> -->
            <!-- 显示前5家公司的按钮 -->
            <el-button
              size="medium"
              round
              v-for="company in firstCompanies"
              :key="company"
              class="company-button"
              ><img
                :src="getIconOfCompany(company)"
                alt=""
                style="width:13px"
              />
              {{ company }}</el-button
            >
            <!-- 显示剩余公司的下拉框列表 -->
            <el-button round size="medium" v-show="hasMore">
              More <i class="el-icon-arrow-down"></i>
            </el-button>
            <div class="popover">
              <el-input
                class="search_bar"
                placeholder="Search Companies"
                size="small"
              ></el-input>
              <ul>
                <li v-for="company in lastCompanies" :key="company">
                  {{ company }}
                </li>
              </ul>
            </div>
          </el-col>
          <el-col :span="8" class="add-salary">
            <el-button class="salary-button" size="medium" round
              >Add Salary</el-button
            >
            <el-button class="salary-button" size="medium" round
              >View Salaries</el-button
            >
          </el-col>
        </el-row>

        <!-- 接下来是显示主内容的表格区 -->
        <el-row>
          <el-col :span="8" style="margin:0 auto;">
            <TableOneColumn></TableOneColumn>
          </el-col>

          <el-col :span="8">
            <TableOneColumn></TableOneColumn>
          </el-col>

          <el-col :span="8">
            <TableOneColumn></TableOneColumn>
          </el-col>
        </el-row>

        <!-- 与TripleByte的推广内容 -->
        <div id="sponsor">
          <a href="https://triplebyte.com/a/OCT5OUw/t1-b">
            <img
              src="../assets/levelsfyi_triplebyte_footer_2340x1428.png"
              alt="Triplebyte: The Fastest Way to Level Up"
            />
          </a>
        </div>

        <!-- 底部栏 -->
        <footer>
          <p class="note">
            Note: Levels are not a hard boundary. Interview performance /
            experience will affect what level you actually fit into. <br />See
            something wrong?
            <a href="mailto:hello@levels.fyi" target="_blank">Let us know.</a>
          </p>

          <p class="craft">
            Crafted by Zaheer & Zuhayeer
          </p>
        </footer>
      </el-col>

      <!-- 表格前后留白 -->
      <el-col :span="2"><div class="grid-content"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import CompanyButtons from './CompanyButtons.vue'
import TableOneColumn from './TableOneColumn.vue'
import axios from 'axios'
export default {
  name: 'MainPage',
  data () {
    return {
      jobOptions: [{
        value: 'Software Engineer',
        label: 'Software Engineer'
      }, {
        value: 'Software Engineering Manager',
        label: 'Software Engineering Manager'
      }, {
        value: 'Product Manager',
        label: 'Product Manager'
      }, {
        value: 'Product Designer',
        label: 'Product Designer'
      }, {
        value: 'Biomedical Engineer',
        label: 'Biomedical Engineer'
      }, {
        value: 'Investment Banking',
        label: 'Investment Banking'
      }, {
        value: 'Civil Engineer',
        label: 'Civil Engineer'
      }],
      selectedJobType: 'Software Engineer', // 界面上选择的工作类型
      selectedCompany: '', // 选择的需要作对比的公司
      companyLevelInfo: {}, // 公司等级信息
      companyInfomation: {}, // 公司网址，logo等信息
      searchCompanyText: '' // 搜索公司的搜索拦
    }
  },
  components: {
    CompanyButtons,
    TableOneColumn
  },

  methods: {
    // 获取某个职位对应的公司数组（原始的职位是一个对象，因此需要将对象中的key提炼出来变成一个数组）
    getKeysOfObject: function (obj) {
      if (obj == null) {
        return []
      }
      var arr = Object.getOwnPropertyNames(obj)
      // 删除"__ob__"
      var idx = arr.indexOf('__ob__')
      console.log('index:', idx)
      if (idx >= 0) {
        arr.splice(idx, 1)
      }

      return arr
    },
    getIconOfCompany: function (company) {
      console.log('getIconOfCompany, company:', company)
      console.log('companyInfomation:', this.companyInfomation)
      var info = this.companyInfomation[company]
      console.log('info:', info)
      if (info != null) {
        console.log('icon:', info.icon)
        return info.icon
      }
      return ''
    }
  },

  created () {
    console.log('hello')
    var _this = this
    axios.get('http://localhost:8080/static/data.json')
      .then(function (response) {
        console.log('data.json:', response)
        _this.companyLevelInfo = response.data
        _this.selectedJobType = 'Software Engineer' // 默认显示的是软件工程师
      })

    axios.get('http://localhost:8080/static/companyInformation.json')
      .then(function (response) {
        console.log('companyInformation:', response)
        _this.companyInfomation = response.data
      })
  },

  computed: {
    // 返回选中工作类型对应的公司中的前5个
    firstCompanies: function () {
      console.log('this.companyLevelInfo:', this.companyLevelInfo)
      var jobTypeCompanies = this.getKeysOfObject(this.companyLevelInfo[this.selectedJobType])
      return jobTypeCompanies.slice(0, 5)
    },
    // 返回选中工作类型对应公司中第5个后面所有的公司
    lastCompanies: function () {
      var jobTypeCompanies = this.getKeysOfObject(this.companyLevelInfo[this.selectedJobType])
      return jobTypeCompanies.slice(5)
    },
    // 判断是否还有更多公司
    hasMore: function () {
      var jobTypeCompanies = this.getKeysOfObject(this.companyLevelInfo[this.selectedJobType])
      var companies = jobTypeCompanies.slice(5)
      if (companies.length > 0) {
        return true
      }

      return false
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

#top-nav {
  background-color: #788b95;
  padding-top: 10px;
  padding-bottom: 30px;
  /* display: inline; */
}

#top-nav .logo img {
  width: 70px;
  /* display:inline */
  vertical-align: -60%;
  /* padding-bottom: 35px; */
}

#top-nav .logo span {
  color: #fff;
  font-size: 3.5em;
  /*  font-family: "Varela Round", sans-serif;
  font-weight: 500;
  letter-spacing: -1px; */
  /* margin-bottom: 20px; */
  /* margin-top: 0px; */
}

#top-nav .sponsor img {
  width: 200px;
}

#top-nav .jobTypeSelect {
  width: 30%;
  margin-top: 0.5em;
}

#top-nav input[type="text"] {
  font-size: 1em;
  font-weight: 700;
  font-family: "Nunito", serif;
}

#sponsor img {
  width: 1170px;
}

#main-content .popover {
  float: right;
  width: 298px;
  margin-top: 10px;
  border: 1px solid;
  overflow: hidden;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  border: 1px solid rgba(0, 0, 0, 0.15);
}

#main-content .popover ul {
  overflow: hidden;
  -webkit-padding-start: 0;
  padding-left: 0;
  cursor: pointer;
}

#main-content .popover li {
  list-style-type: none;
  float: left;
  width: 35%;
  text-align: left;
  padding: 2% 7%;
}

#main-content .popover .search_bar {
  margin: 10px 10px 4px;
  width: 90%;
}

#main-content .popover li:hover {
  background-color: #f4f4f4;
  text-decoration: none;
  color: #333;
}

#main-content .choice-buttons {
  padding: 20px 0 32px 0;
}

#main-content .company-button {
  float: left;
}

#main-content .salary-button {
  float: right;
  margin-left: 10px;
}

footer {
  color: #333;
  font-size: 1em;
  font-weight: 400;
  font-family: "Varela Round", serif;
  margin-top: 10px;
  margin-bottom: 50px;
  text-align: center;
}
footer .note {
  font-size: 85%;
}

footer .craft {
}
</style>
