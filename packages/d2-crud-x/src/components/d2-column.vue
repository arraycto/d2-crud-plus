<template>
  <el-table-column
  :label="handleAttribute(item.title, '')"
  :prop="handleAttribute(item.key, null)"
  v-bind="item">
  <template slot-scope="scope">
    <d2-cell :item="item" :scope="scope" @cell-data-change="handleCellDataChange">
      <template slot-scope="scope" :slot="item.key+'Slot'">
        <slot :name="item.key+'Slot'" :row="scope.row"/>
      </template>
    </d2-cell>
  </template>
    <template v-if="item.children && item.children.length>0">
      <d2-column  v-for="(sub,index) in item.children" :key="index" :item="sub" @cell-data-change="handleCellDataChange">
        <template slot-scope="scope" :slot="sub.key+'Slot'">
          <slot :name="sub.key+'Slot'" :row="scope.row"/>
        </template>
      </d2-column>
    </template>
</el-table-column>
</template>

<script>
import utils from '../mixin/utils'
import D2Cell from './d2-cell'

export default {
  name: 'd2-column',
  mixins: [
    utils
  ],
  components: {
    D2Cell
  },
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  methods: {
    handleCellDataChange (column) {
      this.$emit('cell-data-change', column)
    }
  }
}
</script>
