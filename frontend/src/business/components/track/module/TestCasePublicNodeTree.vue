<template>
  <ms-node-tree class="node-tree"
           v-loading="result.loading"
           local-suffix="test_case"
           default-label="未规划用例"
           @nodeSelectEvent="publicNodeChange"
           :tree-nodes="publicTreeNodes"
           ref="publicNodeTree"/>
</template>

<script>
import MsNodeTree from "@/business/components/track/common/NodeTree";
import {getTestCasePublicNodes} from "@/network/testCase";
export default {
  name: "TestCasePublicNodeTree",
  components: {MsNodeTree},
  props: {
    caseCondition: Object
  },
  data() {
    return {
      publicTreeNodes: [],
      result: {}
    }
  },
  methods: {
    publicNodeChange(node, nodeIds, pNodes) {
      this.$emit("nodeSelectEvent", node, node.data.id === 'root' ? [] : nodeIds, pNodes);
    },
    list() {
      this.result = getTestCasePublicNodes(this.caseCondition, data => {
        this.publicTreeNodes = data;
        if (this.$refs.publicNodeTree) {
          this.publicTreeNodes.forEach(firstLevel => {
              this.$refs.publicNodeTree.nodeExpand(firstLevel);
          })
        }
      });
    },
  }
}
</script>

<style scoped>

</style>
