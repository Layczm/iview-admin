<template>
  <div class="doctor">
    <Card>
      <div class="checkAll">
        <Button type="error"
                @click="deleteCheck">批量删除</Button>
        <Button type="primary"
                class="announce"
                @click="modal1 = true">发布</Button>
      </div>

      <div class="search-con search-con-top">
        <Input @on-change="handleClear"
               clearable
               placeholder="输入关键字搜索"
               class="search-input"
               v-model="searchValue" />
        <Button @click="handleSearch"
                class="search-btn"
                type="primary">
          <Icon type="search" />&nbsp;&nbsp;筛选</Button>
      </div>
      <Table border
             @on-selection-change="onSelectionChange"
             ref="selection"
             :columns="columns7"
             :data="data66"></Table>
      <Page :total="100"
            class="pages"
            show-elevator
            show-sizer />
    </Card>

    <Modal v-model="modal1"
           title="发布"
           width="800"
           :mask-closable="false"
           footer-hide>
      <Form ref="formValidate"
            :model="formValidate"
            :rules="ruleValidate"
            :label-width="80">
        <FormItem label="名称"
                  style="width: 350px"
                  prop="name">
          <Input v-model="formValidate.name"
                 placeholder="请输入姓名"></Input>
        </FormItem>
        <FormItem label="E-mail"
                  style="width: 350px"
                  prop="mail">
          <Input v-model="formValidate.mail"
                 placeholder="请输入邮箱"></Input>
        </FormItem>
        <FormItem label="职称"
                  style="width: 200px"
                  prop="city">
          <Select v-model="formValidate.city"
                  placeholder="请选择职称">
            <Option value="beijing">护士</Option>
            <Option value="shanghai">院长</Option>
            <Option value="shenzhen">经理</Option>
          </Select>
        </FormItem>
        <FormItem label="时间">
          <Row>
            <Col span="8">
            <FormItem prop="date">
              <DatePicker type="date"
                          placeholder="请选择年份"
                          v-model="formValidate.date"></DatePicker>
            </FormItem>
            </Col>
            <Col span="4"
                 style="text-align: center">-</Col>
            <Col span="11">
            <FormItem prop="time">
              <TimePicker type="time"
                          placeholder="请选择时间"
                          v-model="formValidate.time"></TimePicker>
            </FormItem>
            </Col>
          </Row>
        </FormItem>
        <FormItem label="性别"
                  prop="gender">
          <RadioGroup v-model="formValidate.gender">
            <Radio label="male">男</Radio>
            <Radio label="female">女</Radio>
          </RadioGroup>
        </FormItem>
        <FormItem label="标签"
                  prop="interest">
          <CheckboxGroup v-model="formValidate.interest">
            <Checkbox label="Eat"></Checkbox>
            <Checkbox label="Sleep"></Checkbox>
            <Checkbox label="Run"></Checkbox>
            <Checkbox label="Movie"></Checkbox>
          </CheckboxGroup>
        </FormItem>
        <FormItem label="简介"
                  prop="desc">
          <Input v-model="formValidate.desc"
                 type="textarea"
                 :autosize="{minRows: 4,maxRows: 6}"
                 placeholder="请输入内容..."></Input>
        </FormItem>
        <FormItem>
          <Button type="primary"
                  @click="handleSubmit('formValidate')">发布</Button>
          <Button @click="handleReset('formValidate')"
                  style="margin-left: 8px">重置</Button>
        </FormItem>
      </Form>
    </Modal>

    <Modal v-model="examine"
           title="查看"
           width="800"
           footer-hide
           :mask-closable="false">
      <Form ref="formValiUpdata"
            :model="formValiUpdata"
            :rules="ruleValidate"
            :label-width="80">
        <FormItem label="名称"
                  style="width: 350px"
                  prop="name">
          <Input v-model="formValiUpdata.name"
                 placeholder="请输入姓名"></Input>
        </FormItem>
        <FormItem label="E-mail"
                  style="width: 350px"
                  prop="mail">
          <Input v-model="formValiUpdata.mail"
                 placeholder="请输入邮箱"></Input>
        </FormItem>
        <FormItem label="职称"
                  style="width: 200px"
                  prop="city">
          <Select v-model="formValiUpdata.city"
                  placeholder="请选择职称">
            <Option value="beijing">护士</Option>
            <Option value="shanghai">院长</Option>
            <Option value="shenzhen">经理</Option>
          </Select>
        </FormItem>
        <FormItem label="时间">
          <Row>
            <Col span="8">
            <FormItem prop="date">
              <DatePicker type="date"
                          placeholder="请选择年份"
                          v-model="formValiUpdata.date"></DatePicker>
            </FormItem>
            </Col>
            <Col span="4"
                 style="text-align: center">-</Col>
            <Col span="11">
            <FormItem prop="time">
              <TimePicker type="time"
                          placeholder="请选择时间"
                          v-model="formValiUpdata.time"></TimePicker>
            </FormItem>
            </Col>
          </Row>
        </FormItem>
        <FormItem label="性别"
                  prop="gender">
          <RadioGroup v-model="formValiUpdata.gender">
            <Radio label="male">男</Radio>
            <Radio label="female">女</Radio>
          </RadioGroup>
        </FormItem>
        <FormItem label="标签"
                  prop="interest">
          <CheckboxGroup v-model="formValiUpdata.interest">
            <Checkbox label="Eat"></Checkbox>
            <Checkbox label="Sleep"></Checkbox>
            <Checkbox label="Run"></Checkbox>
            <Checkbox label="Movie"></Checkbox>
          </CheckboxGroup>
        </FormItem>
        <FormItem label="简介"
                  prop="desc">
          <Input v-model="formValiUpdata.desc"
                 type="textarea"
                 :autosize="{minRows: 4,maxRows: 6}"
                 placeholder="请输入内容..."></Input>
        </FormItem>
        <FormItem>
          <Button type="primary"
                  @click="modification('formValiUpdata')">保 存</Button>
          <Button @click="cancels"
                  style="margin-left: 8px">取 消</Button>
        </FormItem>
      </Form>
    </Modal>
  </div>
</template>

<script>
export default {
  data () {
    return {
      columns7: [
        {
          type: 'selection',
          width: 60,
          align: 'center'
        },
        {
          title: '名称',
          key: 'name',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.name)
            ])
          }
        },
        {
          title: '出生日期',
          key: 'age',
          sortable: true
        },
        {
          title: '住址',
          key: 'address'
        },
        {
          title: '操作',
          key: 'action',
          width: 150,
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('Button', {
                props: {
                  type: 'primary',
                  size: 'small'
                },
                style: {
                  marginRight: '5px'
                },
                on: {
                  click: () => {
                    this.examine = true
                    // this.show(params.index)
                  }
                }
              }, '查看'),
              h('Button', {
                props: {
                  type: 'error',
                  size: 'small'
                },
                on: {
                  click: () => {
                    this.remove(params.index)
                  }
                }
              }, '删除')
            ])
          }
        }
      ],
      data6: [
        {
          name: '小明',
          age: '2018 - 10 - 10',
          address: '福建省黑龙江XXX市XXX号101'
        },
        {
          name: '小红',
          age: '2018 - 05 - 10',
          address: '福建省四川XXX市XXX号101'
        },
        {
          name: '火龙果',
          age: '2018 - 02 - 15',
          address: '福建省北京市XXX号101'
        },
        {
          name: '香蕉',
          age: '2018 - 01 - 01',
          address: '福建省厦门市集美XXX号101'
        }
      ],
      formValidate: {
        name: '',
        mail: '',
        city: '',
        gender: '',
        interest: [],
        date: '',
        time: '',
        desc: ''
      },
      formValiUpdata: {
        name: '',
        mail: '',
        city: '',
        gender: '',
        interest: [],
        date: '',
        time: '',
        desc: ''
      },
      ruleValidate: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' }
        ],
        mail: [
          { required: true, message: '请输入邮箱', trigger: 'blur' },
          { type: 'email', message: '邮箱格式不正确请重新输入', trigger: 'blur' }
        ],
        city: [
          { required: true, message: '请选择职称', trigger: 'change' }
        ],
        gender: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ],
        interest: [
          { required: true, type: 'array', min: 1, message: 'Choose at least one hobby', trigger: 'change' },
          { type: 'array', max: 2, message: 'Choose two hobbies at best', trigger: 'change' }
        ],
        date: [
          { required: true, type: 'date', message: '请选择日期', trigger: 'change' }
        ],
        time: [
          { required: true, type: 'string', message: '请选择时间', trigger: 'change' }
        ],
        desc: [
          { required: true, message: '请输入内容', trigger: 'blur' },
          { type: 'string', min: 20, message: '内容不少于20个单词', trigger: 'blur' }
        ]
      },
      selectionData: [],
      modal1: false,
      examine: false,
      searchValue: ''
    }
  },
  methods: {
    show (index) {
      this.$Modal.info({
        title: 'User Info',
        content: `Name：${this.data6[index].name}<br>Age：${this.data6[index].age}<br>Address：${this.data6[index].address}`
      })
    },
    remove (index) {
      this.data6.splice(index, 1)
    },
    onSelectionChange (selection) {
      this.selectionData = selection
    },
    deleteCheck () {
      console.log(this.selectionData)
      if (!this.selectionData.length) {
        // this.$Modal.warning({
        //   title: '提示',
        //   content: '<p>请选择要删除的内容</p>'
        // })
        this.$Message.warning('请选择要删除的内容')
      } else {
        this.$Modal.confirm({
          title: '提示',
          content: '<p>此操作将永久删除该文件, 是否继续?</p>',
          onOk: () => {
            this.$Message.info('删除成功')
          },
          onCancel: () => {
            this.$Message.info('已取消删除')
          }
        })
      }
    },
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$Message.success('发布成功')
          this.modal1 = false
        } else {
          this.$Message.error('请填完整以下内容')
        }
      })
    },
    modification (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$Message.success('修改成功')
          this.examine = false
        } else {
          this.$Message.error('请填完整以下内容')
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    cancels () {
      this.examine = false
      this.$Message.info('已取消修改')
    },
    // 搜索
    handleSearch () {
      console.log('搜索')
    },
    handleClear (e) {
      // console.log(e.target.value)
      console.log('输入内容')
    }
  },
  computed: {
    data66 () {
      const { searchValue, data6 } = this
      if (searchValue) {
        return data6.filter(function (dataNews) {
          return Object.keys(dataNews).some(function (key) {
            return String(dataNews[key]).toLowerCase().indexOf(searchValue) > -1
          })
        })
      }
      return data6
    }
  }
}
</script>

<style lang="less" scoped>
.doctor {
  width: 100%;
  height: 100%;
  .pages {
    margin: 50px 0;
    text-align: center;
  }
  .search-con {
    padding: 10px 0;
    display: inline-block;
    .search {
      &-col {
        display: inline-block;
        width: 200px;
      }
      &-input {
        display: inline-block;
        width: 200px;
        margin-left: 2px;
      }
      &-btn {
        margin-left: 2px;
      }
    }
  }

  .checkAll {
    display: inline-block;

    .announce {
      margin: 0 10px 0 10px;
      width: 80px;
    }
  }
}
</style>
