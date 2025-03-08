<template>
  <div class="centered-toolbar">
    <div class="tool-group">
      <el-button-group>
        <el-button :type="currentTool === 'move' ? 'primary' : 'default'" @click="setTool('move')">
          <el-icon><Pointer /></el-icon>
        </el-button>
        <el-button :type="currentTool === 'pen' ? 'primary' : 'default'" @click="setTool('pen')">
          <el-icon><EditPen /></el-icon>
        </el-button>
      </el-button-group>

      <el-divider direction="vertical" />

      <el-color-picker v-model="localColor" size="small" @change="updateColor($event || '')" />
      <el-select
        v-model="localLineWidth"
        size="small"
        :placeholder="$t('drawingBoard.lineWidth')"
        @change="updateLineWidth"
      >
        <el-option
          v-for="width in lineWidthOptions"
          :key="width"
          :label="width + 'px'"
          :value="width"
        />
      </el-select>

      <el-divider direction="vertical" />

      <el-button size="small" type="success" @click="saveImage">
        <el-icon><Download /></el-icon>
        {{ $t('drawingBoard.saveAsPNG') }}
      </el-button>

      <el-button size="small" type="danger" @click="clearDrawing">
        <el-icon><Delete /></el-icon>
        {{ $t('drawingBoard.clearDrawing') }}
      </el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref, defineProps, defineEmits, watch } from 'vue'
  import { Pointer, EditPen, Download, Delete } from '@element-plus/icons-vue'
  import { DrawingTool } from './types'

  const props = defineProps({
    currentTool: {
      type: String as () => DrawingTool,
      default: 'pen'
    },
    color: {
      type: String,
      default: '#000000'
    },
    lineWidth: {
      type: Number,
      default: 2
    }
  })

  const emit = defineEmits([
    'update:currentTool',
    'update:color',
    'update:lineWidth',
    'save',
    'clear-drawing'
  ])

  const lineWidthOptions = [1, 2, 3, 5, 8, 12]
  const localColor = ref(props.color)
  const localLineWidth = ref(props.lineWidth)

  // ����props�仯
  watch(
    () => props.color,
    (newVal) => {
      localColor.value = newVal
    }
  )

  watch(
    () => props.lineWidth,
    (newVal) => {
      localLineWidth.value = newVal
    }
  )

  // ���ù���
  const setTool = (tool: DrawingTool) => {
    emit('update:currentTool', tool)
  }

  // ������ɫ
  const updateColor = (color: string) => {
    emit('update:color', color)
  }

  // �����߿�
  const updateLineWidth = (width: number) => {
    emit('update:lineWidth', width)
  }

  // ����ͼ��
  const saveImage = () => {
    emit('save')
  }

  // ����滭����
  const clearDrawing = () => {
    emit('clear-drawing')
  }
</script>

<style lang="scss" scoped>
  .centered-toolbar {
    position: fixed;
    top: 20px;
    left: 50%;
    z-index: 100;
    display: flex;
    align-items: center;
    padding: 8px;
    background-color: #2d2d2d;
    border-radius: 8px;
    box-shadow: 0 2px 12px rgb(0 0 0 / 30%);
    transform: translateX(-50%);

    .tool-group {
      display: flex;
      gap: 8px;
      align-items: center;

      :deep(.el-button-group) {
        .el-button {
          padding: 8px;
          color: #e0e0e0;
          background-color: #383838;
          border-color: #4d4d4d;

          &:hover {
            background-color: #404040;
          }

          &.is-active,
          &.el-button--primary {
            color: #fff;
            background-color: #4a4a9c;
            border-color: #4a4a9c;
          }

          .el-icon {
            font-size: 16px;
          }
        }
      }

      :deep(.el-divider) {
        height: 24px;
        margin: 0 4px;
        border-color: #4d4d4d;
      }

      :deep(.el-color-picker) {
        margin: 0 4px;
      }

      :deep(.el-select) {
        width: 80px;

        .el-input {
          background-color: #383838;

          .el-input__wrapper {
            padding: 0 8px;
            background-color: #383838;
            box-shadow: 0 0 0 1px #4d4d4d inset;

            &:hover {
              box-shadow: 0 0 0 1px #666 inset;
            }
          }

          input {
            height: 32px;
            font-size: 12px;
            color: #e0e0e0;
          }
        }
      }

      :deep(.el-button) {
        height: 32px;
        padding: 8px;
        color: #e0e0e0;
        background-color: #383838;
        border-color: #4d4d4d;

        &:hover {
          background-color: #404040;
        }

        &.el-button--primary {
          color: #fff;
          background-color: #4a4a9c;
          border-color: #4a4a9c;
        }

        .el-icon {
          font-size: 16px;
        }
      }
    }
  }
</style>
