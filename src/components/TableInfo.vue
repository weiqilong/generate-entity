<template>
  <div>
    <!-- 生成按钮 -->
    <el-button type="primary" @click="generateJavaClass"
      >第二步-生成代码</el-button
    >

    <el-table
      :data="convertTables"
      stripe
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <!-- <el-table-column type="selection" :selectable="selectable" width="55" /> -->
      <el-table-column type="selection" width="55" />
      <el-table-column prop="id" label="ID" width="180"></el-table-column>
      <el-table-column prop="name" label="Table Name"></el-table-column>
    </el-table>
  </div>
</template>

<script setup lang="ts">
import { ComputedRef, computed } from "vue";

// 传入数组
const props = defineProps<{ tables: string[] }>();

let selectTables: string[] = [];

// 定义一个类型，类型有id,name
interface TableItem {
  id: number;
  name: string;
}
const convertTables: ComputedRef<TableItem[]> = computed(() => {
  return props.tables.map((table, index) => {
    // 返回TableItem类型的对象
    return {
      id: index,
      name: table,
    };
  });
});

const handleSelectionChange = (tableItem: TableItem[]) => {
  // 把tableItem获取name变成数组，赋值给selectTables
  selectTables = tableItem.map((item) => item.name);
  // 打印selectTables
  console.log(selectTables);
};

const generateJavaClass = async () => {
  const response = await fetch("http://127.0.0.1:8080/generate/createClass", {
    method: "POST",
    body: JSON.stringify(selectTables),
    headers: {
      "Content-Type": "application/json",
    },
  });
  // 判断状态码，如果200，则提示成功
  if (response.status === 200) {
    alert("生成成功");
  } else {
    alert("生成失败");
  }
};
</script>

<style scoped></style>
