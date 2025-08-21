<template>
  <div>
    <!-- 基本用法 -->
    <el-dialog
      title="对话框标题"
      v-model="dialogVisible"
      :show-close="true"
      :close-on-click-modal="false"
      :close-on-press-escape="false"
      center
      width="50%"
    >
      <div>这里是对话框内容</div>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="dialogVisible = false">确认</el-button>
        </span>
      </template>
    </el-dialog>

    <!-- 可拖动对话框 -->
    <div v-if="dragDialogVisible" v-drag="['.drag-dialog.el-dialog', '.drag-dialog .el-dialog__header']">
      <el-dialog
        :title="i18nt('designer.toolbar.preview')"
        v-model="dragDialogVisible"
        :show-close="true"
        class="drag-dialog"
        :append-to-body="true"
        center
      >
        <div>可拖动对话框内容</div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      dragDialogVisible: false
    }
  },
  methods: {
    i18nt(key) {
      // 国际化方法示例
      return key
    }
  }
}
</script>

<style scoped>
.dialog-footer {
  text-align: center;
}
</style>