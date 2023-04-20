<script setup lang="ts">
import { exampleSetup } from "prosemirror-example-setup";
import { DOMParser, Schema } from "prosemirror-model";
import { schema } from "prosemirror-schema-basic";
import { addListNodes } from "prosemirror-schema-list";
import { EditorState } from "prosemirror-state";
import { EditorView } from "prosemirror-view";

import { onMounted, ref } from "vue";

let editorRef = ref<HTMLElement>();

onMounted(() => {

  // 创建一个新的  ProseMirror 文档规范对象 docSchema
  // Schema: 对编辑器 dom 结构、规则的抽象描述的对象，类似 编辑器DOM 转换为 JS Object 之前先定义 JS Object 结构的过程
  // 是一个抽象描述 编辑器实例 DOM 结构和规则的对象，可以理解为编辑器 dom 的 js 对象模板，它规定了 编辑器 dom 内可出现的  node 类型、属性、样式，以及 node 之间的关系
  let docSchema = new Schema({
    nodes: addListNodes(schema.spec.nodes as any, "paragraph block*", "block"),
    marks: schema.spec.marks,
  })
  // 默认支持的节点类型
  //  文本节点
  //  块级节点
  // 内联节点
    console.log("🚀 ~ file: App.vue:22 ~ onMounted ~ schema:", schema)
  //  docSchema 
  //  markFromJSON
  // nodeFromJSON
  console.log("🚀 ~ file: App.vue:22 ~ onMounted ~ docSchema:", docSchema)
  // @ts-ignore
  let state = EditorState.create({
    schema,
    doc: DOMParser.fromSchema(docSchema).parse(editorRef.value as HTMLElement),
    plugins: exampleSetup({ schema: docSchema, menuBar: false })
  });
  let view = new EditorView(editorRef.value as HTMLElement, {
    state,
  });

  editorRef.value?.focus()
});
</script>   

<template>
  <div id="editor" ref="editorRef" contenteditable></div>
</template>
<style lang="css">
@import url("./css/reset.css");
@import url("/node_modules/prosemirror-view/style/prosemirror.css");
@import url("/node_modules/prosemirror-menu/style/menu.css");
@import url("/node_modules/prosemirror-gapcursor/style/gapcursor.css");
@import url("/node_modules/prosemirror-example-setup/style/style.css");

.ProseMirror {
  width: 210mm;
  height: 297mm;
}
</style>
<style scoped>
#editor {
  width: 220mm;
  height: 50vh;
  overflow-y: scroll;
  padding: 1em 2em;
}
</style>

