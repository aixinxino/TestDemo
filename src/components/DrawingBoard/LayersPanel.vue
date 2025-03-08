<template>
  <div class="layers-panel">
    <div class="panel-header">
      <h3>{{ $t('drawingBoard.layers') }}</h3>
      <el-button type="primary" size="small" @click="addNewLayer">
        <el-icon><Plus /></el-icon>
      </el-button>
    </div>

    <div class="layers-list">
      <div
        v-for="(layer, index) in layers"
        :key="layer.id"
        class="layer-item"
        :class="{ active: activeLayerIndex === index }"
        @click="selectLayer(index)"
      >
        <div class="layer-info">
          <el-switch
            v-model="layer.visible"
            inline-prompt
            :active-icon="View"
            :inactive-icon="Hide"
            @change="toggleLayerVisibility(index)"
          />
          <div class="layer-preview">
            <canvas :ref="(el) => setLayerPreviewRef(el, layer.id)"></canvas>
          </div>
          <span class="layer-name">{{ layer.name }}</span>
        </div>

        <div class="layer-actions">
          <div v-if="layer.id === 'background'" class="color-text" @click.stop="toggleColorPicker">
            {{ backgroundColor.toUpperCase() }}
          </div>
          <el-button v-else size="small" circle @click.stop="deleteLayer(index)">
            <el-icon><Delete /></el-icon>
          </el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref, defineProps, defineEmits, onMounted, watch, onUnmounted } from 'vue'
  import { Plus, View, Hide, Delete } from '@element-plus/icons-vue'
  import { Layer } from './types'

  const props = defineProps({
    layers: {
      type: Array as () => Layer[],
      required: true
    },
    activeLayerIndex: {
      type: Number,
      default: 0
    },
    backgroundColor: {
      type: String,
      default: '#ffffff'
    }
  })

  const emit = defineEmits([
    'add-layer',
    'select-layer',
    'toggle-visibility',
    'clear-layer',
    'toggle-color-picker',
    'delete-layer'
  ])

  // ͼ��Ԥ����������
  const layerPreviews = ref<Map<string, HTMLCanvasElement>>(new Map())

  const setLayerPreviewRef = (el: any, layerId: string) => {
    if (el && el instanceof HTMLCanvasElement) {
      layerPreviews.value.set(layerId, el)
    }
  }

  // ������ͼ��
  const addNewLayer = () => {
    emit('add-layer')
  }

  // ѡ��ͼ��
  const selectLayer = (index: number) => {
    emit('select-layer', index)
  }

  // �л�ͼ��ɼ���
  const toggleLayerVisibility = (index: number) => {
    emit('toggle-visibility', index)
  }

  // ɾ��ͼ��
  const deleteLayer = (index: number) => {
    emit('delete-layer', index)
  }

  // �л���ɫѡ����
  const toggleColorPicker = () => {
    emit('toggle-color-picker')
  }

  // ����ͼ��仯������Ԥ��
  watch(
    () => props.layers,
    () => {
      updateLayerPreviews()
    },
    { deep: true }
  )

  onMounted(() => {
    updateLayerPreviews()
    window.addEventListener('resize', updateLayerPreviews)
  })

  onUnmounted(() => {
    window.removeEventListener('resize', updateLayerPreviews)
  })

  // ����ͼ��Ԥ��
  const updateLayerPreviews = () => {
    props.layers.forEach((layer) => {
      const canvas = layerPreviews.value.get(layer.id)
      if (canvas) {
        const ctx = canvas.getContext('2d')
        if (ctx) {
          // �������
          ctx.clearRect(0, 0, canvas.width, canvas.height)

          // ����Ǳ���ͼ�㣬��䱳��ɫ
          if (layer.id === 'background') {
            ctx.fillStyle = props.backgroundColor
            ctx.fillRect(0, 0, canvas.width, canvas.height)
          }

          // ��������
          if (layer.visible) {
            layer.lines.forEach((line) => {
              if (line.points.length < 2) return

              ctx.beginPath()
              ctx.moveTo(line.points[0].x, line.points[0].y)

              for (let i = 1; i < line.points.length; i++) {
                ctx.lineTo(line.points[i].x, line.points[i].y)
              }

              ctx.strokeStyle = line.color
              ctx.lineWidth = line.width
              ctx.lineCap = 'round'
              ctx.lineJoin = 'round'
              ctx.stroke()
            })
          }
        }
      }
    })
  }
</script>

<style lang="scss" scoped>
  .layers-panel {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 10;
    display: flex;
    flex-direction: column;
    width: 240px;
    height: calc(100vh - 40px);
    margin: 20px;
    overflow: hidden;
    color: #fff;
    background-color: #1e1e1e;
    border-right: none;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgb(0 0 0 / 30%);

    .panel-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 16px;
      background-color: #2d2d2d;
      border-bottom: 1px solid #000;

      h3 {
        margin: 0;
        font-size: 14px;
        font-weight: 500;
        color: #fff;
      }

      :deep(.el-button) {
        height: 24px;
        min-height: 24px;
        padding: 4px;
        background-color: transparent;
        border: none;

        &:hover {
          background-color: #404040;
        }

        .el-icon {
          font-size: 14px;
        }
      }
    }

    .layers-list {
      flex: 1;
      height: calc(100% - 50px);
      padding: 8px;
      overflow-y: auto;
      background-color: #1e1e1e;

      .layer-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 36px;
        padding: 6px 8px;
        margin: 4px 0;
        cursor: pointer;
        background-color: #2d2d2d;
        border-radius: 8px;
        transition: all 0.2s;

        &.active {
          background-color: #4a4a9c;
        }

        &:hover:not(.active) {
          background-color: #383838;
        }

        .layer-info {
          display: flex;
          flex: 1;
          gap: 6px;
          align-items: center;

          .layer-preview {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 28px;
            height: 28px;
            overflow: hidden;
            background-color: #fff;
            border: 1px solid #4d4d4d;
            border-radius: 3px;

            canvas {
              width: 100%;
              height: 100%;
              background-color: transparent;
            }
          }

          .layer-name {
            flex: 1;
            overflow: hidden;
            font-size: 12px;
            color: #e0e0e0;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
        }

        .layer-actions {
          position: relative;
          display: flex;
          gap: 4px;
          align-items: center;

          .color-text {
            position: relative;
            padding: 2px 4px;
            font-family: monospace;
            font-size: 11px;
            color: #e0e0e0;
            cursor: pointer;
            background-color: transparent;
            border-radius: 2px;
          }

          :deep(.el-switch) {
            --el-switch-on-color: #4a4a9c;
            --el-switch-off-color: #383838;

            margin-right: -4px;
            transform: scale(0.7);
          }

          :deep(.el-button) {
            height: 24px;
            min-height: 24px;
            padding: 4px;
            background-color: transparent;
            border: none;

            &:hover {
              background-color: #404040;
            }

            .el-icon {
              font-size: 14px;
            }
          }
        }
      }
    }
  }
</style>
