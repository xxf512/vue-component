<template>
  <div>
    <tree-node
      v-for="(item , index) in clonData"
      :key="index"
      :data="item"
      :show-checkbox="showCheckbox"
    ></tree-node>
  </div>
</template>

<script>
import TreeNode from './node'
import { deepCopy } from '../../utils/assist.js'

export default {
  name: 'Tree',
  props: {
    data: {
      type: Array,
      default: () => {
        return []
      }
    },
    showCheckbox: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      clonData: []
    }
  },

  components: {
    TreeNode
  },

  watch: {
    data () {
      this.rebuildData()
    }
  },

  computed: {},

  created () {
    this.rebuildData()
  },

  mounted () {},

  methods: {
    rebuildData () {
      this.clonData = deepCopy(this.data)
    },
    emitEvent (eventName, data) {
      this.$emit(eventName, data, this.clonData)
    }
  }
}

</script>
<style lang='less' scoped>
</style>
