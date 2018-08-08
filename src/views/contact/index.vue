<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input @keyup.enter.native="filterMajor"
                style="width: 200px;"
                class="filter-item"
                :placeholder="name"
                v-model="major">
      </el-input>
      <el-select clearable
                 style="width: 150px"
                 class="filter-item"
                 v-model="tableData.major"
                 :placeholder="major">
        <el-option v-for="item in major"
                   :key="item"
                   :label="item"
                   :value="item">
        </el-option>
      </el-select>
      <!-- <el-select clearable class="filter-item" style="width: 130px" v-model="listQuery.type" :placeholder="null">
        <el-option v-for="item in  calendarTypeOptions" :key="item.key" :label="item.display_name+'('+item.key+')'" :value="item.key">
        </el-option>
      </el-select>
      <el-select @change='handleFilter' style="width: 140px" class="filter-item" v-model="listQuery.sort">
        <el-option v-for="item in sortOptions" :key="item.key" :label="item.label" :value="item.key">
        </el-option>
      </el-select> -->
      <el-button class="filter-item"
                 type="primary"
                 v-waves
                 icon="el-icon-search"
                 @click="handleFilter">{{'搜索'}}</el-button>
      <el-button class="filter-item"
                 style="margin-left: 10px;"
                 type="primary"
                 icon="el-icon-edit">{{'添加'}}</el-button>
      <el-button class="filter-item"
                 type="primary"
                 v-waves
                 :loading="downloadLoading"
                 icon="el-icon-download">{{'导出'}}</el-button>
      <!-- <el-checkbox class="filter-item"
                   style='margin-left:15px;'
                   @change='tableKey=tableKey+1'
                   v-model="showReviewer">{{'审核人'}}</el-checkbox> -->
    </div>
    <el-table :data="tableData"
              style="width: 100%">
      <el-table-column prop="date"
                       label="日期"
                       sortable
                       width="180"
                       :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
                       :filter-method="filterHandler">
        <template slot-scope="scope">
          {{tableData[scope.$index]['date']}}
        </template>
      </el-table-column>
      <el-table-column prop="name"
                       label="姓名"
                       width="180">
        <template slot-scope="scope">
          {{tableData[scope.$index]['name']}}
        </template>
      </el-table-column>
      <el-table-column prop="major"
                       label="专业"
                       :filters="[{text: '电子商务', value: '电子商务'},{text: '软件技术', value: '软件技术'},{text: '数字媒体', value: '数字媒体'}
                       ,{text: '网络技术', value: '网络技术'},{text: '软件与信息服务', value: '软件与信息服务'}]"
                       :filter-method="filterMajor">
        <template slot-scope="scope">
          {{tableData[scope.$index]['major']}}
        </template>
      </el-table-column>
      <el-table-column prop="class"
                       label="班级">
        <template slot-scope="scope">
          {{tableData[scope.$index]['class']}}
        </template>
      </el-table-column>
      <el-table-column prop="QQ"
                       label="QQ">
        <template slot-scope="scope">
          {{tableData[scope.$index]['QQ']}}
        </template>
      </el-table-column>
      <el-table-column prop="address"
                       label="地址"
                       :formatter="formatter">
        <template slot-scope="scope">
          {{tableData[scope.$index]['address']}}
        </template>
      </el-table-column>
      <el-table-column prop="grade"
                       label="年级"
                       :filters="[{ text: '大一', value: '大一' }, { text: '大二', value: '大二' },{ text: '老师', value: '老师' }]"
                       :filter-method="filterGrade"
                       filter-placement="bottom-end">
        <template slot-scope="scope">
          <el-tag :type="scope.row.grade === '大一' ? 'primary' : 'success'"
                  disable-transitions>{{scope.row.grade}}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import res from './res.js'
import waves from '../../directive/waves' // 水波纹指令
// import request from '../../utils/request'
// export function fetchList(query) {
//   return request({
//     url: './res.js',
//     method: 'get',
//     params: query
//   })
// }
// console.log(fetchList())
export default {
  name: 'Contact',
  directives: {
    waves
  },
  data() {
    return {
      date: null,
      name: null,
      major: null,
      class: null,
      QQ: null,
      address: null,
      grade: null,
      tableData: [
      ],
      downloadLoading: true
    }
  },
  created: function() {
    for (const i of res) {
      // var obj = {}
      // obj.date = res.data[i].date
      // obj.name = res.data[i].name
      // obj.address = res.data[i].address
      this.tableData.push(i)
    }
  },
  methods: {
    formatter(row, column) {
      return row.address
    },
    filterGrade(value, row) {
      return row.grade === value
    },
    filterHandler(value, row, column) {
      const property = column['property']
      return row[property] === value
    },
    filterMajor(value, row) {
      return row.major === value
    },
    handleFilter() {
      this.listQuery.page = 1
      this.getList()
    }
  }
}
</script>