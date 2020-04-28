<template>
  <div id="app">
    <el-select-tree v-model="value" :selectOpt="option.select" :treeOpt="option.tree" @remove-tag="removeTreeSelect" @check="handleTreeCheckChange" @node-click="clickNode">
      <template v-slot:tree="{node}">
        <span :title="node.label">{{node.label}}</span>
      </template>
    </el-select-tree>
  </div>
</template>

<script>
import ElSelectTree from '@/components/ElSelectTree.vue'

export default {
  name: 'App',
  components: {
    ElSelectTree
  },
  data () {
    return {
      value: '',
      option: {
        select: {
          filterable: true,
          clearable: true
        },
        tree: {
          highlightCurrent: true,
          data: [
            {
              label: '一级 1',
              children: [{
                label: '二级 1-1',
                children: [{
                  label: '三级 1-1-1'
                }]
              }]
            }, {
              label: '一级 2',
              children: [{
                label: '二级 2-1',
                children: [{
                  label: '三级 2-1-1'
                }]
              }, {
                label: '二级 2-2',
                children: [{
                  label: '三级 2-2-1'
                }]
              }]
            }, {
              label: '一级 3',
              children: [{
                label: '二级 3-1',
                children: [{
                  label: '三级 3-1-1'
                }]
              }, {
                label: '二级 3-2',
                children: [{
                  label: '三级 3-2-1'
                }]
              }]
            }
          ],
          expandOnClickNode: false,
          nodeKey: 'label',
          props: {},
          indent: 10,
          displayKey: 'label',
          showCheckbox: true
        }
      }
    }
  },
  methods: {
    removeTreeSelect () {

    },
    handleTreeCheckChange () {

    },
    clickNode () {

    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
