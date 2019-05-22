<template>
  <div>
    <Card>
      <tables ref="tables" editable searchable search-place="top" v-model="tableData" :columns="columns" @on-delete="handleDelete" @on-edit="handleEdit"/>
      <div class="kk3">
        <div class="kk"> 
          <Button style="margin: 10px 0;" type="primary" @click="exportExcel">导出为Csv文件</Button>
        </div>
        <div class="kk2">
          <Page :total="0" size="small" show-elevator show-sizer></Page>
        </div>
      </div>
    </Card>
  </div>
</template>
<style>
 .dd
 {
   border: 1px solid red;
   height: 100px;
   width: 100%;
 }
 .kk
 {
   float: left;
   height: 50px;
   width: 50%;
   /* border: 1px solid rgb(212, 209, 209); */
 }
 .kk2
 {
   float: right;
   height: 50px;
   width: 50%;
   /* border: 1px solid rgb(0, 255, 128); */
   line-height: 55px;
   text-align: right;
   padding-right: 10px;
 }
 .kk3
 {
   height: 50px;
 }
</style>

<script>
import Tables from '_c/tables'
import { getTableData } from '@/api/data'
import tableform from './table-form'
export default {
  name: 'tables_page',
  components: {
    Tables,
    tableform
  },
  data () {
    return {
      columns: [
        { title: 'Name', key: 'name', sortable: true },
        { title: 'Email', key: 'email', editable: true },
        { title: 'Create-Time', key: 'createTime' },
        {
          title: 'Handle',
          key: 'handle',
          options: ['delete', 'edit'],
          button: [
            (h, params, vm) => {
              return h('Poptip', {
                props: {
                  confirm: true,
                  title: '你确定要删除吗?'
                },
                on: {
                  'on-ok': () => {
                    vm.$emit('on-delete', params)
                    vm.$emit('input', params.tableData.filter((item, index) => index !== params.row.initRowIndex))
                  }
                }
              }, [
                h('Button', '自定义删除')
              ])
            }
          ]
        }
      ],
      tableData: []
    }
  },
  methods: {
    handleDelete (params) {
      console.log(params)
    },
    handleEdit (params) {
      console.log(params)
      this.$Modal.confirm({
        title: '第一个窗口',
        render: h => {
          return h(tableform, {
            ref: 'tableform',
            props: {
              confirm: true,
              item: {
                input: '111test',
                select: 'shanghai',
                radio: 'female',
                checkbox: ['吃饭','跑步'],
                switch: true,
                date: '2019-10-07',
                time: '15:30:30',
                slider: [10, 50],
                textarea: '我是简介'
              }
            }
          })
        },
        width: 600,
        closable: false,
        okText: '确定',
        cancelText: '取消',
        //loading: true,
        onOk (param1,param2) {
          console.log('测试OK')
          console.log(this.$refs.tableform.formItem)
          console.log(this.$refs.tableform.formItem.textarea)
        }
      })
    },
    exportExcel () {
      this.$refs.tables.exportCsv({
        filename: `table-${(new Date()).valueOf()}.csv`
      })
    }
  },
  mounted () {
    getTableData().then(res => {
      this.tableData = res.data
    })
  }
}
</script>

<style>

</style>
