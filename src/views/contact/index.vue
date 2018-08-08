<template>
  <div class="app-container">
    <el-table :data="tableData"
              style="width: 100%">
      <el-table-column prop="date"
                       label="日期"
                       sortable
                       width="180"
                       :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
                       :filter-method="filterHandler">
      </el-table-column>
      <el-table-column prop="name"
                       label="姓名"
                       width="180">
      </el-table-column>
      <el-table-column prop="address"
                       label="地址"
                       :formatter="formatter">
      </el-table-column>
      <el-table-column prop="grade"
                       label="年级"
                       width="100"
                       :filters="[{ text: '大一', value: '大一' }, { text: '大二', value: '大二' },{ text: '老师', value: '老师' }]"
                       :filter-method="filterGrade"
                       filter-placement="bottom-end">
        <template slot-scope="scope">
          <el-grade :type="scope.row.grade === '大一' ? 'primary' : 'success'"
                  disable-transitions>{{scope.row.grade}}</el-grade>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [{
        date: '2016-05-02',
        name: '潘苗绒',
        address: '上海市普陀区金沙江路 1518 弄',
        grade: '大一'
      }, {
        date: '2016-05-04',
        name: '张楠',
        address: '上海市普陀区金沙江路 1517 弄',
        grade: '大二'
      }, {
        date: '2016-05-01',
        name: '张浩',
        address: '上海市普陀区金沙江路 1519 弄',
        grade: '老师'
      }, {
        date: '2016-05-03',
        name: '刘向华',
        address: '上海市普陀区金沙江路 1516 弄',
        grade: '老师'
      }]
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
    }
  }
}
</script>