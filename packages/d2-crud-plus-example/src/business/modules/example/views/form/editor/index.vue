<template>
  <d2-container>
    <template slot="header">富文本
      <example-helper title="帮助" >
        <div>
          <link-button href="http://greper.gitee.io/d2-crud-plus/guide/extends/full-editor.html">富文本扩展</link-button>
        </div>
      </example-helper>

    </template>
    <crud-search ref="search" :options="crud.searchOptions" @submit="handleSearch"  >
    </crud-search>
    <d2-crud
        ref="d2Crud"
        :columns="crud.columns"
        :data="crud.list"
        :rowHandle="crud.rowHandle"
        edit-title="修改"
        :add-template="crud.addTemplate"
        :add-rules="crud.addRules"
        :edit-template="crud.editTemplate"
        :edit-rules="crud.editRules"
        :form-options="crud.formOptions"
        :options="crud.options"
        :loading="crud.loading"
        :indexRow="crud.indexRow"
        @dialog-open="handleDialogOpen"
        @row-edit="handleRowEdit"
        @row-add="handleRowAdd"
        @row-remove="handleRowRemove"
        @dialog-cancel="handleDialogCancel"
        @form-data-change="handleFormDataChange"
        @form-component-ready="handleFormComponentReady"
    >
      <el-button slot="header" class="d2-mb-5" size="small" type="primary" @click="addRow">新增</el-button>

      <template slot="expandSlot" slot-scope="scope">
        这里显示行展开数据:{{scope.row.data}}
      </template>

    </d2-crud>
    <crud-footer ref="footer" :current="crud.page.current"
                  :size="crud.page.size"
                  :total="crud.page.total"
                  @change="handlePaginationChange"
    >
    </crud-footer>
  </d2-container>
</template>

<script>
import { AddObj, GetObj, GetList, UpdateObj, DelObj } from './api'
import { crudOptions } from './crud'
import { d2CrudPlus } from 'd2-crud-plus'
import helper from './helper'
export default {
  name: 'formEditor',
  components: {},
  mixins: [d2CrudPlus.crud],
  data () {
    return {
      helper: helper
    }
  },
  created () {
    // 配置编辑前获取详情
    this.crud.options.fetchDetail = this.fetchDetail
  },
  computed: {
  },
  methods: {
    getCrudOptions () {
      return crudOptions(this)
    },
    pageRequest (query) {
      return GetList(query)
    },
    infoRequest (query) {
      return GetObj(query)
    },
    addRequest (row) {
      return AddObj(row)
    },
    updateRequest (row) {
      return UpdateObj(row)
    },
    delRequest (row) {
      return DelObj(row.id)
    },
    // 编辑对话框打开前获取详情
    fetchDetail (index, row) {
      return GetObj(row.id).then(res => {
        this.changeEditor(res.data.change)
        return res.data
      })
    },
    changeEditor (value) {
      if (value === 'quill') {
        this.getEditFormTemplate('content').component.show = true
        this.getEditFormTemplate('content_ueditor').component.show = false
      } else {
        this.getEditFormTemplate('content').component.show = false
        this.getEditFormTemplate('content_ueditor').component.show = true
      }
    },
    handleFormComponentReady (event, key, form) {
      console.log('form component ready:', event, key, form)
    }
  }
}
</script>
