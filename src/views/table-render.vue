<template>
  <div>
    <table-render :columns="columns" :data="data">
      <!-- slot-scope测试 -->
      <template slot-scope="{ row, index }" slot="test">
        <input type="text" v-model="editTest" v-if="editIndex === index" />
        <span v-else>{{ row.test }}</span>
      </template>
    </table-render>
  </div>
</template>
<script>
import TableRender from '../components/table-render/table.vue'

export default {
  components: { TableRender },
  data () {
    let that = this
    return {
      editName: '', // 第一列输入框
      editAge: '', // 第二列输入框
      editBirthday: '', // 第三列输入框
      editAddress: '', // 第四列输入框
      editTest: '', // 第五列输入框
      editIndex: -1, // 当前聚焦的输入框的行数
      columns: [
        {
          title: '姓名',
          key: 'name',
          render: (params) => {
            const { row, index } = params
            return (
              <div>
                {
                  this.editIndex === index
                    ? <input
                      onInput={(e) => {
                        this.editName = event.target.value
                      }}
                      value={row.name}/>
                    : row.name
                }
              </div>
            )
          }
        },
        {
          title: '年龄',
          key: 'age',
          render: (params) => {
            const { row, index } = params
            return (
              <div>
                {
                  this.editIndex === index
                    ? <input
                      onInput={(e) => {
                        this.editAge = event.target.value
                      }}
                      value={row.age}/>
                    : row.age
                }
              </div>
            )
          }
        },
        {
          title: '出生日期',
          key: 'birthday',
          // render: (params) => {
          //   const { row } = params
          //   const date = new Date(parseInt(row.birthday))
          //   return <span>{that.formatDateDT(date, 'yyyy-MM-dd')}</span>
          // }
          render: (params) => {
            const { row, index } = params
            const date = new Date(parseInt(row.birthday))
            const birthday = that.formatDateDT(date, 'yyyy-MM-dd')
            return (
              <div>
                {
                  this.editIndex === index
                    ? <input
                      onInput={(e) => {
                        this.editBirthday = event.target.value
                      }}
                      value={row.birthday}/>
                    : <span>{birthday}</span>
                }
              </div>
            )
          }
        },
        {
          title: '地址',
          key: 'address',
          render: (params) => {
            const { row, index } = params
            return (
              <div>
                {
                  this.editIndex === index
                    ? <input
                      onInput={(e) => {
                        this.address = event.target.value
                      }}
                      value={row.address}/>
                    : row.address
                }
              </div>
            )
          }
        },
        {
          title: 'slot-scope测试',
          slot: 'test'
        },
        {
          title: '操作',
          render: (params) => {
            const { row, index } = params
            return (
              <div>
                {
                  this.editIndex === index
                    ? (
                      <div>
                        <button
                          style='margin-right: 10px;'
                          onClick={() => {
                            this.data[index].name = this.editName
                            this.data[index].age = this.editAge
                            this.data[index].birthday = this.editBirthday
                            this.data[index].address = this.editAddress
                            this.data[index].test = this.editTest
                            this.editIndex = -1
                          }}>保存</button>
                        <button onClick={() => { this.editIndex = -1 }}>取消</button>
                      </div>
                    )
                    : <button onClick={() => {
                      this.editName = row.name
                      this.editAge = row.age
                      this.editAddress = row.address
                      this.editBirthday = row.birthday
                      this.editTest = row.test
                      this.editIndex = index
                    }}>修改</button>
                }
              </div>
            )
          }
        }
      ],
      data: [
        {
          name: '王小明',
          age: 18,
          birthday: '919526400000',
          test: '王小明 slot-scope测试',
          address: '北京市朝阳区芍药居'
        },
        {
          name: '张小刚',
          age: 25,
          birthday: '696096000000',
          test: '张小刚 slot-scope测试',
          address: '北京市海淀区西二旗'
        },
        {
          name: '李小红',
          age: 30,
          test: '李小红 slot-scope测试',
          birthday: '563472000000',
          address: '上海市浦东新区世纪大道'
        },
        {
          name: '周小伟',
          age: 26,
          test: '周小伟 slot-scope测试',
          birthday: '687024000000',
          address: '深圳市南山区深南大道'
        }
      ]
    }
  },
  methods: {
    formatDateDT (date, fmt) {
      if (!date) return ''
      if (!(date instanceof Date)) {
        try {
          date = new Date(date)
        } catch (error) {
          console.error('时间格式有误，请检查')
        }
      }
      var o = {
        'M+': date.getMonth() + 1, // 月份
        'd+': date.getDate(), // 日
        'h+': date.getHours(), // 小时
        'm+': date.getMinutes(), // 分
        's+': date.getSeconds(), // 秒
        'q+': Math.floor((date.getMonth() + 3) / 3), // 季度
        'S': date.getMilliseconds() // 毫秒
      }
      if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
      for (var k in o) {
        if (new RegExp('(' + k + ')').test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
      }
      return fmt
    }
  }
}
</script>
