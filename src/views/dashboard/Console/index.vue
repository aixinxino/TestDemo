<template>
  <div class="home-container">
    <!-- 顶部操作栏 -->
    <div class="header">
      <div class="left">
        <h2>Home</h2>
      </div>
      <div class="right">
        <el-button type="primary" plain>Invite Members</el-button>
        <el-button>New folder</el-button>
        <el-button type="primary" @click="handleNewFile">New file</el-button>
      </div>
    </div>

    <!-- 模板区域 -->
    <div class="templates-section">
      <div class="section-header">
        <h3>Get started with templates</h3>
        <el-button text @click="toggleTemplates">
          {{ showTemplates ? 'Hide' : 'Show' }} Templates
        </el-button>
      </div>

      <div v-if="showTemplates" class="templates-grid">
        <div class="template-card" v-for="template in templates" :key="template.id">
          <el-card :body-style="{ padding: '0px' }">
            <div class="template-image">
              <img :src="template.image" />
            </div>
            <div class="template-info">
              <span>{{ template.name }}</span>
              <el-button type="primary" text>Use template</el-button>
            </div>
          </el-card>
        </div>
      </div>
    </div>

    <!-- 最近文件区域 -->
    <div class="recent-section">
      <h3>Recent</h3>
      <div class="recent-grid">
        <el-card v-for="file in recentFiles" :key="file.id" class="recent-card">
          <div class="file-preview">
            <img :src="file.preview" alt="File preview" />
          </div>
          <div class="file-info">
            <span class="file-name">{{ file.name }}</span>
            <span class="file-date">{{ file.date }}</span>
          </div>
        </el-card>
      </div>
    </div>

    <!-- 所有文件列表 -->
    <div class="files-section">
      <div class="section-header">
        <h3>All files</h3>
        <el-dropdown>
          <span class="el-dropdown-link">
            Last edited
            <el-icon class="el-icon--right"><arrow-down /></el-icon>
          </span>
        </el-dropdown>
      </div>

      <el-table :data="allFiles" style="width: 100%">
        <el-table-column prop="name" label="Name">
          <template #default="{ row }">
            <div class="name-cell">
              <span>{{ row.name }}</span>
            </div>
          </template>
        </el-table-column>
        <el-table-column prop="created" label="Created" />
        <el-table-column prop="editedBy" label="Edited by">
          <template #default="{ row }">
            <div class="user-cell">
              <span>{{ row.editedBy }}</span>
            </div>
          </template>
        </el-table-column>
        <el-table-column prop="location" label="Location">
          <template #default="{ row }">
            <div class="location-cell">
              <el-tag size="small" type="info" effect="plain">
                {{ row.location }}
              </el-tag>
            </div>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'
  import { ArrowDown } from '@element-plus/icons-vue'
  import { useRouter } from 'vue-router'
  import { useUserStore } from '@/store/modules/user'

  const router = useRouter()
  const userStore = useUserStore()

  const showTemplates = ref(true)
  const toggleTemplates = () => {
    showTemplates.value = !showTemplates.value
  }

  const handleNewFile = () => {
    // 检查用户是否已登录
    if (!userStore.isLogin) {
      // 未登录则跳转到登录页
      router.push('/login')
    } else {
      // 已登录则跳转到画板页面
      router.push('/drawing-board')
    }
  }

  const templates = ref([
    {
      id: 1,
      name: 'Palettes Basics',
      image: '/templates/palette.jpg'
    }
  ])

  const recentFiles = ref([
    {
      id: 1,
      name: 'Palettes Basics',
      date: 'about 9 hours ago',
      preview: '/previews/palette.jpg'
    },
    {
      id: 2,
      name: 'Untitled',
      date: 'about 21 hours ago',
      preview: '/previews/untitled.jpg'
    }
  ])

  const allFiles = ref([
    {
      name: 'Untitled',
      created: 'March 6th, 2025',
      editedBy: 'xxx',
      userAvatar: '/avatars/user1.jpg',
      preview: '/previews/untitled.jpg',
      location: 'All Workspace'
    }
  ])
</script>

<style lang="scss">
  @use './index.scss';
</style>
