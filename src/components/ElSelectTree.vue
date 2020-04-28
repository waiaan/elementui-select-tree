<template>
  <el-select :value="value" class="el-select-tree" v-bind="selectOpt" ref="select" :multiple="multiple" value-key="label" @clear="clear" @remove-tag="removeTag">
    <el-option style="display:none" value="0">
    </el-option>
    <el-tree ref="tree" v-bind="treeOpt" @check="handleTreeCheckChange" @node-click="clickNode">
      <span slot-scope="{ node, data }">
        <slot name="tree" v-bind="{node,data}">
          <span :title="node.label">{{node.label}}</span>
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
  computed: {
    multiple () {
      return this.treeOpt.showCheckbox;
    },
    displayKey () {
      const { displayKey } = this.treeOpt;
      if (!displayKey) {
        throw new Error('need "displayKey" in treeOpt');
      }
      return displayKey
    }
  },
  methods: {
    setValue (nodes) {
      const value = [];
      for (let i = 0; i < nodes.length; i++) {
        const { children } = nodes[i];
        if (children) continue;
        value.push(nodes[i][this.displayKey]);
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
        const value = data[this.displayKey]
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
            if (this.value.indexOf(node[this.displayKey]) > -1) {
              this.$refs.tree.setChecked(node, false);
            }
          }
          this.$emit('change', []);
        } else {
          this.$emit('change', '');
        }
        this.$emit('clear')
      }
    },
    removeTag (label) {
      const checkedNodes = this.$refs.tree.getCheckedNodes();
      for (let i = 0; i < checkedNodes.length; i++) {
        const node = checkedNodes[i];
        if (node[this.displayKey] === label) {
          this.$refs.tree.setChecked(node, false);
        }
      }
      this.setValue(this.$refs.tree.getCheckedNodes());
      this.$emit('remove-tag', label);
    }
  }
}
</script>
<style scoped>
</style>
