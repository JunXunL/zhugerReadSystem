<template>
  <div>
    <myNav :bread="routerData"/>
    <div class="table-page">
      <my-table
        :list="list"
        :options="options"
        :columns="columns"
        :operates="operates"
        :pagination="ruleForm"
        @handleSizeChange="handleSizeChange"
        @handleCurrentChange="handleCurrentChange"
      />

    </div>
  </div>
</template>
<script>
import myTable from '@/components/myTable'
import { getStageList, delCheckpoint } from '@/api/book'
export default {
  components: { myTable },
  data() {
    return {
      bookId: '',
      bookName: '',
      routerData: {
        content: [{
          url: { path: '/books/section/index', query: { }},
          name: '章节管理'
        }, {
          url: { path: '/books/addStage/index', query: { }},
          name: '添加新关卡'
        }]
      },
      list: [],
      options: {
        stripe: true, // 是否为斑马纹 table
        loading: false, // 是否添加表格loading加载动画
        highlightCurrentRow: true, // 是否支持当前行高亮显示
        mutiSelect: false // 是否支持列表项选中功能
      },
      columns: [ // table 的参数
        {
          prop: 'id',
          label: '序号',
          align: 'center'
        },
        {
          prop: 'checkpoint_title',
          label: '关卡标题',
          align: 'center'
        },
        {
          prop: 'chipAward',
          label: '碎片奖励',
          align: 'center'
        },
        {
          prop: 'word_num',
          label: '字数',
          align: 'center'
        }

      ], // 需要展示的列
      operates: {
        width: 300,
        fixed: 'right',
        list: [
          {
            id: '1',
            label: '编辑',
            style: 'color:#4e8afd;font-size:13px;',
            method: (index, row) => {
              this.$router.push({ path: '/books/addStage/index', query: { pointId: row.id, bookId: this.bookId, bookName: this.bookName }})
            }
          },
          {
            id: '2',
            label: '删除',
            style: 'color:#fb9697;font-size:13px;',
            method: (index, row) => {
              this.handleDel(index, row)
            }
          }
        ]
      }, // 列操作按钮
      ruleForm: {
        page: 1,
        pagesize: 10,
        total: 0
      }
    }
  },
  created() {
    this.bookId = this.$route.query.bookId
    this.bookName = this.$route.query.bookName
    this.routerData.content.forEach((item, index) => {
      item.url.query.bookId = this.bookId
      item.url.query.bookName = this.bookName
    })
    this.routerData.route = `书籍管理 > 关卡 (${this.bookName})`
    this.fetchData()
  },
  methods: {
    fetchData() {
      getStageList({ book_id: this.bookId, page: this.ruleForm.page, pagesize: this.ruleForm.pagesize }).then(res => {
        res.data.data.forEach(function(item, index) {
          var chipAward = ''
          item.drop_detail.forEach(function(ele, i) {
            chipAward += `${ele.companion_name}*${ele.chip_num}`
            if (i != item.drop_detail.length - 1) {
              chipAward += '，'
            }
          })
          item.chipAward = chipAward
        })
        this.list = res.data.data
        this.ruleForm.total = +res.data.count
      })
    },
    // 删除
    handleDel(index, row) {
      delCheckpoint({ id: row.id }).then(res => {
        this.$message.success('删除成功')
        this.fetchData()
      })
    },
    handleSizeChange(val) {
      this.ruleForm.pagesize = val
      this.ruleForm.page = val
      this.fetchData()
    },
    handleCurrentChange(val) {
      this.ruleForm.page = val
      this.fetchData()
    }
  }
}
</script>
<style scoped>

</style>

