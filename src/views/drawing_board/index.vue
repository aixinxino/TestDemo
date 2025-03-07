<template>
  <div class="drawing-board">
    <div class="header">
      <div class="left">
        <h2>Drawing Board</h2>
      </div>
      <div class="right">
        <el-button type="primary">Save</el-button>
        <el-button>Share</el-button>
      </div>
    </div>

    <div class="canvas-container">
      <!-- 这里后续会添加画布组件 -->
      <div class="canvas-placeholder">
        <el-empty description="Start drawing your masterpiece" />
      </div>
    </div>
  </div>
</template>

<script setup>
  // 后续会添加画布相关的逻辑
</script>

<style lang="scss" scoped>
  .drawing-board {
    min-height: 100vh;
    background-color: var(--bg-color);
    color: var(--text-color);
    padding: 20px 24px;

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;

      h2 {
        margin: 0;
        font-size: 24px;
        font-weight: 500;
      }

      .right {
        display: flex;
        gap: 12px;
      }
    }

    .canvas-container {
      background-color: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      min-height: calc(100vh - 120px);
      display: flex;
      align-items: center;
      justify-content: center;

      .canvas-placeholder {
        text-align: center;
      }
    }
  }
</style>
