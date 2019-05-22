<template>
  <div>
    <Card>
      <tables ref="tables" editable searchable search-place="top" v-model="tableData" :columns="columns" @on-delete="handleDelete" @on-edit="handleEdit"/>
      <Button style="margin: 10px 0;" type="primary" @click="exportExcel">导出为Csv文件</Button>
    </Card>
  </div>
</template>

<script>
import Tables from '_c/tables'
import { getTableData } from '@/api/data'
import tableform from './table-form'
import { truncate } from 'fs';
import { lookup } from 'dns';
export default {
  name: 'tables_page',
  components: {
    Tables
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
          console.log(param1)
          console.log(param2)
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
