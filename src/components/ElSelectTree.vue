<template>
  <el-select ref="select" :value="selectVal" class="el-select-tree" popper-class="el-select-tree-dropdown" v-bind="selectOpt" :multiple="multiple" value-key="label" @clear="clear" @remove-tag="removeTag">
    <el-option style="display:none" value="0" />
    <el-tree ref="tree" v-bind="treeOpt" @check="handleTreeCheckChange" @node-click="clickNode">
      <span slot-scope="{ node, data }">
        <slot name="tree" v-bind="{node,data}">
          <span :title="node.label">{{ node.label }}</span>
        </slot>
      </span>
    </el-tree>
  </el-select>
</template>
<script>
export default {
  name: 'ElSelectTree',
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: [String, Object, Array],
    selectOpt: {
      type: Object,
      required: true
    },
    treeOpt: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      selectVal: ''
    }
  },
  computed: {
    multiple () {
      return this.treeOpt.showCheckbox;
    },
    valueKey () {
      const { valueKey } = this.treeOpt;
      if (!valueKey) {
        throw new Error('need "valueKey" in treeOpt');
      }
      return valueKey
    }
  },
  methods: {
    setValue (nodes) {
      const value = [];
      for (let i = 0; i < nodes.length; i++) {
        const { children } = nodes[i];
        if (children) continue;
        value.push(nodes[i][this.valueKey]);
      }
      this.$emit('change', value)
    },
    handleTreeCheckChange (data, tree) {
      const { checkedNodes } = tree;
      this.setValue(checkedNodes);
      this.$emit('check', data, tree);
    },
    clickNode (data, node, treeNode) {
      if (!this.multiple) {
        const value = data[this.valueKey]
        this.selectVal = data.label
        this.$refs.select.blur();
        this.$emit('change', value)
      }
      this.$emit('node-click', data, node, treeNode);
    },
    clear () {
      if (this.selectOpt.clearable) {
        if (this.multiple) {
          const checkedNodes = this.$refs.tree.getCheckedNodes();
          for (let i = 0; i < checkedNodes.length; i++) {
            const node = checkedNodes[i];
            if (this.value.indexOf(node[this.valueKey]) > -1) {
              this.$refs.tree.setChecked(node, false);
            }
          }
          this.selectVal = []
          this.$emit('change', []);
        } else {
          this.selectVal = ''
          this.$emit('change', '');
        }
        this.$emit('clear')
      }
    },
    removeTag (label) {
      const checkedNodes = this.$refs.tree.getCheckedNodes();
      for (let i = 0; i < checkedNodes.length; i++) {
        const node = checkedNodes[i];
        if (node[this.valueKey] === label) {
          this.$refs.tree.setChecked(node, false);
        }
      }
      this.setValue(this.$refs.tree.getCheckedNodes());
      this.$emit('remove-tag', label);
    }
  }
}
</script>
<style lang="scss">
.el-select-tree-dropdown .el-tree {
  padding: 0 8px;
}
</style>
