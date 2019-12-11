<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper" >
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">

          <a-col :md="6" :sm="12">
            <a-form-item label="姓名">
              <j-input placeholder="输入姓名模糊查询" v-model="queryParam.name"></j-input>
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
          <a-form-item label="年龄">
            <!-- <a-input placeholder="请输入名称查询" v-model="queryParam.age"></a-input>-->
            <a-input placeholder="最小年龄" type="ge" v-model="queryParam.age_begin" style="width:calc(50% - 15px);"></a-input>
            <span class="group-query-strig">~</span>
            <a-input placeholder="最大年龄" type="le" v-model="queryParam.age_end" style="width:calc(50% - 15px);"></a-input>
          </a-form-item>
          </a-col>

          <a-col :md="4" :sm="8">
            <a-form-item label="性别">
              <a-select v-model="queryParam.sex" placeholder="请选择性别查询">
                <a-select-option value="">请选择性别查询</a-select-option>
                <a-select-option value="1">男性</a-select-option>
                <a-select-option value="2">女性</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>

          <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
            <a-col :md="6" :sm="24">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </a-col>
          </span>


        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <div class="table-operator" style="border-top: 5px">

      <a-button @click="handleAdd" type="primary" icon="plus">新增学生</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('学生信息')" style="margin-left: 8px">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import" style="margin-left: 8px">导入</a-button>
      </a-upload>

    </div>



    <div>
      <a-table
        ref="table"
        bordered
        size="middle"
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical"/>
          <a-dropdown>
            <a class="ant-dropdown-link">
              更多 <a-icon type="down"/>
            </a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>

           </a-menu>
          </a-dropdown>
        </span>
       </a-table>
    </div>

    <!-- 表单区域 -->
    <student-modal ref="modalForm" @ok="modalFormOk"></student-modal>

    </a-card>
</template>

<script>
  import StudentModal from './modules/StudentModal'
  import {putAction} from '@/api/manage';
  import {JeecgListMixin} from '@/mixins/IanfgListMixin';
  import JInput from '@/components/jeecg/JInput';

  export default {
    name: "UserList",
    mixins: [JeecgListMixin],
    components: {
      StudentModal,
      JInput
    },
    data() {
      return {
        description: '这是学生管理页面',
        queryParam: {},
        //表头
        columns:[],
        //列设置
        settingColumns:[],
        columns: [
          {
            title: '姓名',
            align: "center",
            dataIndex: 'name',
            width: 120
          },
          {
            title: '性别',
            align: "center",
            dataIndex: 'sex_dictText'
          },
          {
            title: '年龄',
            align: "center",
            dataIndex: 'age'
          },
          {
            title: '邮箱',
            align: "center",
            dataIndex: 'email'
          },
          {
            title: '个人简介',
            align: "center",
            dataIndex: 'content'
          },
          {
            title: '操作',
            dataIndex: 'action',
            scopedSlots: {customRender: 'action'},
            align: "center",
            width: 170
          }
        ],
        url: {
          list: "/student/list",
          delete: "/student/delete",
          exportXlsUrl: "/student/exportXls",
          importExcelUrl: "/student/importExcel",
        },
      }
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      getAvatarView: function (avatar) {
        return this.url.imgerver + "/" + avatar;
      },
      jump() {
      }

    },
    created() {
    },
  }
</script>

<style scoped>
</style>