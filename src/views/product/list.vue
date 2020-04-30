<template>
  <div>
    <!-- 筛选查询条件-->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="商品编号">
              <a-input v-model="queryParam.id" placeholder="" :allowClear="true"/>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="商品状态">
              <a-select v-model="queryParam.status" placeholder="请选择" default-value="0" :allowClear="true">
                <a-select-option value="0">上架中</a-select-option>
                <a-select-option value="1">已下架</a-select-option>
                <a-select-option value="2">待上架</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <template v-if="advanced">
            <a-col :md="8" :sm="24">
              <a-form-item label="商品名称">
                <a-input-number v-model="queryParam.callNo" style="width: 100%" :allowClear="true"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="更新日期">
                <a-date-picker v-model="queryParam.date" style="width: 100%" placeholder="请输入更新日期" :allowClear="true"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="使用状态">
                <a-select placeholder="请选择" default-value="0" :allowClear="true">
                  <a-select-option value="0">全部</a-select-option>
                  <a-select-option value="1">关闭</a-select-option>
                  <a-select-option value="2">运行中</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
          </template>
          <a-col :md="!advanced && 8 || 24" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
              <a-button type="primary" @click="$refs.table.refresh(true)">查询</a-button>
              <a-button style="margin-left: 8px" @click="() => queryParam = {}">重置</a-button>
              <a @click="toggleAdvanced" style="margin-left: 8px">
                {{ advanced ? '收起' : '展开' }}
                <a-icon :type="advanced ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 操作选项 -->
    <div class="table-operator">
      <a-button type="primary" icon="plus" @click="handleAdd()">新建</a-button>
    </div>
    <!-- 表格数据 -->
    <a-table :columns="columns"  :data-source="data" border>
      <span slot="tags" slot-scope="tags">
        <a-tag
          v-for="tag in tags"
          :key="tag"
          :color="tag === 'loser' ? 'volcano' : tag.length > 5 ? 'geekblue' : 'green'"
        >
        {{ tag.toUpperCase() }}
      </a-tag>
      </span>
      <span slot="action" slot-scope="text, record">
        <a>失效-{{ record.name }}</a>
        <a-divider type="vertical" />
        <a>删除</a>
        <a-divider type="vertical" />
        <a class="ant-dropdown-link"> 更多操作 <a-icon type="down" /> </a>
      </span>
    </a-table>

  </div>
</template>

<script>
  const columns = [
    {title: 'ID', dataIndex: 'key'},
    {title: '名称', dataIndex: 'name'},
    {title: '年龄', dataIndex: 'age'},
    {title: '地址', dataIndex: 'address'},
    {title: 'tags', dataIndex: 'tags', scopedSlots: { customRender: 'tags' }},
    {title: 'Action', key: 'action', scopedSlots: { customRender: 'action' }},
  ]


  export default {
    name: 'ProductListPage',
    data () {
      return {
        // 高级搜索 展开/关闭
        advanced: false,
        // 查询参数
        queryParam: {},
        columns,
        data: [
          {
            key: '1',
            name: 'John Brown',
            age: 32,
            address: 'New York No. 1 Lake Park',
            tags: ['nice', 'developer'],
          },
          {
            key: '2',
            name: 'Jim Green',
            age: 42,
            address: 'London No. 1 Lake Park',
            tags: ['loser'],
          },
          {
            key: '3',
            name: 'Joe Black',
            age: 32,
            address: 'Sidney No. 1 Lake Park',
            tags: ['cool', 'teacher'],
          },
        ]
      }
    },
    methods: {
      toggleAdvanced() {
        this.advanced = !this.advanced
      },
      handleAdd() {
        this.$message.info('新建操作')
      }
    }
  }
</script>

<style scoped>
</style>
