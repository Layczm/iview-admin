<template>
    <div>
        <Card>
            <v-table :columns="columns"
                     searchable
                     v-model="tableData"></v-table>
        </Card>
    </div>
</template>

<script>
import vTable from '_c/tables'

export default {
  data () {
    return {
      columns: [
        { title: '名称', key: 'name', sortable: true },
        { title: '邮箱', key: 'age', editable: true },
        { title: '创建时间', key: 'address' },
        {
          title: 'Handle',
          key: 'handle',
          options: ['delete'],
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
      tableData: [
        {
          name: '王小明',
          age: 18,
          address: '北京市朝阳区芍药居'
        },
        {
          name: '张小刚',
          age: 25,
          address: '北京市海淀区西二旗'
        },
        {
          name: '李小红',
          age: 30,
          address: '上海市浦东新区世纪大道'
        },
        {
          name: '周小伟',
          age: 26,
          address: '深圳市南山区深南大道'
        }
      ]
    }
  },
  components: {
    vTable
  }
}
</script>
