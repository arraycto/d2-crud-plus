<template>
  <span>
    <template v-if="type === 'text'">
      <span v-for="(item) in items" :key="item.url" >
        <el-link type="primary" size="mini" :underline="false" :href="item.url"  target="_blank"> {{item.name}} </el-link>
      </span>
    </template>
    <template v-else >
      <el-tag class='tag-item  d2-mr-5 d2-mb-2 d2-mt-2' v-for="(item) in items" :key="item.url"  size="small"  :type="item.color"  >
        <el-link type="primary"  :underline="false" :href="item.url"  target="_blank">{{item.name}}</el-link>
      </el-tag>
    </template>
  </span>
</template>

<script>
// 文件格式化展示组件
export default {
  name: 'd2p-files-format',
  props: {
    // 值
    value: {
      require: true
    },
    // 颜色，【primary, success, warning, danger ,info】
    color: {
      require: false,
      default: 'primary'
    },
    // 展示类型【text, tag】
    type: {
      default: 'tag' // 可选【text,tag】
    }
  },
  data () {
    return {
    }
  },
  computed: {
    items () {
      if (this.value == null || this.value === '') {
        return []
      }
      let valueArr = []
      if (typeof (this.value) === 'string') {
        valueArr = [this.getItem(this.value)]
      } else if (this.value instanceof Array) {
        // 本来就是数组的
        valueArr = []
        for (let val of this.value) {
          valueArr.push(this.getItem(val))
        }
      }
      return valueArr
    }
  },
  created () {
  },
  methods: {
    getFileName (url) {
      if (url.lastIndexOf('/') >= 0) {
        return url.substring(url.lastIndexOf('/') + 1)
      }
      return url
    },
    getItem (value) {
      return {
        url: value,
        name: this.getFileName(value),
        color: this.color
      }
    }
  }
}
</script>
<style >
  .d2-mb-2{margin-bottom: 2px}
  .d2-mt-2{margin-top: 2px;}
  .tag-item{
    margin-right: 10px;
  }
  .el-divider__text, .el-link {
    font-size: inherit;
  }
</style>
