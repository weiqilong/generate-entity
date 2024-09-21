<script setup lang="ts">
import { Ref, reactive, ref } from "vue";
import TableInfo from "./TableInfo.vue";

const formData = reactive({
  host: "127.0.0.1",
  port: "3306",
  username: "root",
  password: "123456",
  database: "dev",
  outputDir: "C:\\Users\\weiqi\\Desktop\\xxx",
  packageName: "com.weiqilong.demo",
  author: "小宝",
  generateDeleteField: true,
  deleteFieldName: "delete_flag",
  ignorePrefix: "t_",
  generateMapperService: true,
  generateInsertFields: "create_time,create_user",
  generateUpdateFields: "update_time,update_user",
});

const tables: Ref<string[]> = ref([]);

// 方法submitForm
const submitForm = async () => {
  // 在这里执行表单提交逻辑，例如发送请求到服务器
  const response = await fetch("http://127.0.0.1:8080/generate/tables", {
    method: "POST",
    body: JSON.stringify(formData),
    headers: {
      "Content-Type": "application/json",
    },
  });
  // 返回内容赋值给tables
  tables.value = await response.json();
  console.log(tables);
};
</script>

<template>
  <div>
    <el-form :model="formData" label-width="auto" style="max-width: 600px">
      <h3>数据库配置</h3>
      <el-form-item label="host:" required>
        <el-input v-model="formData.host" />
      </el-form-item>
      <el-form-item label="端口号:" required>
        <el-input v-model="formData.port" />
      </el-form-item>
      <el-form-item label="用户名:" required>
        <el-input v-model="formData.username" />
      </el-form-item>
      <el-form-item label="密码:" required>
        <el-input v-model="formData.password" />
      </el-form-item>
      <el-form-item label="数据库:" required>
        <el-input v-model="formData.database" />
      </el-form-item>
      <h3>常规配置</h3>
      <el-form-item label="生成目录:" required>
        <el-input v-model="formData.outputDir" />
      </el-form-item>
      <el-form-item label="包名:" required>
        <el-input v-model="formData.packageName" />
      </el-form-item>
      <el-form-item label="作者:" required>
        <el-input v-model="formData.author" />
      </el-form-item>

      <el-tooltip content="使用逻辑删除注解,会增加@TableLogic" placement="top">
        <el-form-item label="是否使用逻辑删除注解">
          <el-switch v-model="formData.generateDeleteField" />
        </el-form-item>
      </el-tooltip>
      <el-tooltip content="使用逻辑删除注解,会增加@TableLogic" placement="top">
        <el-form-item v-if="formData.generateDeleteField" label="逻辑删除字段:">
          <el-input v-model="formData.deleteFieldName" />
        </el-form-item>
      </el-tooltip>

      <!-- insert策略注解字段，鼠标悬停提示 -->
      <el-tooltip
        content="insert策略注解字段,@TableField(fill = FieldFill.INSERT),默认为create_time,create_user,多个字段用,隔开"
        placement="top"
      >
        <el-form-item label="insert策略注解字段">
          <el-input
            v-model="formData.generateInsertFields"
            placeholder="多个字段用,隔开"
          />
        </el-form-item>
      </el-tooltip>
      <!-- update策略注解字段，鼠标悬停提示 -->
      <el-tooltip
        content="update策略注解字段,@TableField(fill = FieldFill.INSERT_UPDATE),默认为update_time,update_user,多个字段用,隔开"
        placement="top"
      >
        <el-form-item label="update策略注解字段">
          <el-input
            v-model="formData.generateUpdateFields"
            placeholder="多个字段用,隔开"
          />
        </el-form-item>
      </el-tooltip>

      <el-form-item label="忽略表前缀:">
        <el-input v-model="formData.ignorePrefix" />
      </el-form-item>
      <el-form-item label="是否生成mapper和service类" required>
        <el-switch v-model="formData.generateMapperService" />
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm"
          >第一步-更新配置</el-button
        >
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
    <h3>表信息</h3>
    <TableInfo :tables="tables"></TableInfo>
  </div>
</template>

<style scoped></style>
