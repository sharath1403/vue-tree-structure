<template>
  <draggable class="dragArea" tag="div" :list="nodes">
    <div
      v-for="node in nodes"
      :key="node.name"
      :style="{ 'margin-left': `${depth * 20}px` }"
      class="node"
    >
      <div class="selectedRow">
        <span class="type" @click="nodeClicked(node)">{{
          isExpanded(node)
            ? "&#8722;"
            : node.children !== undefined
            ? "&#43;"
            : ""
        }}</span>
        <span class="type">&#128447;</span>
        <span :style="getStyle(node)" @click="checktheclickedNode(node)">{{
          node.name
        }}</span>
      </div>
      <TreeBrowser
        v-if="isExpanded(node) && node.children"
        :nodes="node.children"
        :depth="depth + 1"
        @onClick="(node) => $emit('onClick', node)"
      />
    </div>
  </draggable>
</template>

<script>
import draggable from "vuedraggable";

import * as ColorHash from "color-hash";
const colorHash = new ColorHash();

export default {
  name: "TreeBrowser",
  props: {
    nodes: Array,
    depth: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      expanded: [],
    };
  },
  methods: {
    isExpanded(node) {
      return this.expanded.indexOf(node) !== -1;
    },
    nodeClicked(node) {
      if (!this.isExpanded(node)) {
        this.expanded.push(node);
      } else {
        this.expanded.splice(this.expanded.indexOf(node));
      }
    },
    checktheclickedNode(node) {
      console.log(node.name);
    },
    getStyle(node) {
      let color = "red";
      let cursor = "pointer";
      if (!node.children) {
        color = colorHash.hex(node.name.split(".")[1]);
      }
      return {
        //color,
        cursor,
      };
    },
    getImgUrl() {
      return require("./../assets/plus.png");
    },
  },
  components: {
    draggable,
  },
  computed: {},
};
</script>

<style scoped>
.node {
  text-align: left;
  font-size: 18px;
}

.type {
  margin-right: 10px;
  cursor: pointer;
}

/* .selectedRow {
} */
</style>
