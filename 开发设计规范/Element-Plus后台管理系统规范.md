# Element Plus 后台管理系统设计规范

本文档结合了两份规范，定义了基于 Element Plus 组件库的页面设计标准，确保系统界面视觉一致、交互流畅。

## 一、组件规范

### 1.1 表格组件规范

**组件要求：**
- 必须使用 `el-table` + `el-pagination` 组合
- 操作列必须包含 `查看`、`编辑`、`删除` 按钮
- 删除操作必须使用 `el-popconfirm` 包裹

**布局规范：**
- 工具栏使用 `.table-toolbar` 左右分离布局
- 操作列按钮顺序：查看 → 编辑 → 删除
- 分页器默认使用 `layout="total, sizes, prev, pager, next"`

**样式规则：**
- 表格必须启用 `border` 和 `stripe`
- 行高默认 54px，紧凑模式 48px
- 操作列按钮使用 `link` 类型

**代码示例：**
```vue
<template>
  <div class="table-container">
    <!-- 工具栏 -->
    <div class="table-toolbar">
      <div class="left">
        <el-button type="primary" @click="handleAdd">新增</el-button>
        <el-button type="danger" @click="handleBatchDelete">批量删除</el-button>
      </div>
      <div class="right">
        <el-input v-model="searchKeyword" placeholder="请输入关键词" clearable>
          <template #append>
            <el-button icon="Search" @click="handleSearch" />
          </template>
        </el-input>
      </div>
    </div>
    
    <!-- 表格 -->
    <el-table 
      :data="tableData" 
      border 
      stripe
      v-loading="tableLoading"
      @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55" />
      <el-table-column prop="date" label="日期" />
      <el-table-column prop="name" label="名称" />
      <el-table-column prop="status" label="状态" />
      <el-table-column label="操作" width="220" fixed="right">
        <template #default="{ row }">
          <el-button link @click="handleView(row)">查看</el-button>
          <el-button link type="primary" @click="handleEdit(row)">编辑</el-button>
          <el-popconfirm title="确认删除该条数据？" @confirm="handleDelete(row)">
            <template #reference>
              <el-button link type="danger">删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    
    <!-- 分页 -->
    <div class="pagination-container">
      <el-pagination
        v-model:current-page="pagination.currentPage"
        v-model:page-size="pagination.pageSize"
        :page-sizes="[10, 20, 50, 100]"
        layout="total, sizes, prev, pager, next"
        :total="pagination.total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>
```

### 1.2 表单组件规范

**组件要求：**
- 搜索表单使用 `el-form` + `inline` 属性
- 详情表单使用 `el-form` + `label-position="right"`
- 表单按钮统一放置在表单底部，居右对齐

**样式规则：**
- 表单项标签宽度统一设置为 `label-width="100px"`
- 必填项标签前添加红色星号 `required` 属性
- 表单项之间保持16px间距

**代码示例：**
```vue
<template>
  <el-form
    ref="formRef"
    :model="form"
    :rules="rules"
    label-width="100px"
    label-position="right"
  >
    <el-form-item label="用户名" prop="username" required>
      <el-input v-model="form.username" placeholder="请输入用户名" />
    </el-form-item>
    
    <el-form-item label="角色" prop="role">
      <el-select v-model="form.role" placeholder="请选择角色">
        <el-option
          v-for="item in roleOptions"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
    </el-form-item>
    
    <el-form-item>
      <div class="form-buttons">
        <el-button @click="resetForm">取消</el-button>
        <el-button type="primary" @click="submitForm" :loading="submitLoading">提交</el-button>
      </div>
    </el-form-item>
  </el-form>
</template>
```

## 二、整体布局规范

### 2.1 页面结构

系统采用经典的"头部-侧边栏-内容区"三段式布局：

- **头部区域**：高度固定为 60px，包含 logo、系统名称、全局搜索、用户信息等
- **侧边栏**：固定宽度 210px，显示菜单导航
- **内容区**：自适应宽度，包含页面标题、筛选区、内容展示区等

### 2.2 间距与留白

- **页面外边距**：内容区域与容器边缘间距 20px
- **区块间距**：相邻内容区块之间保持 16px-24px 间隔
- **元素间距**：同一区块内部元素间距 12px
- **内部填充**：内容容器内部填充 16px-20px

## 三、筛选区域规范

### 3.1 筛选区结构

- 筛选区使用 `el-card` 组件包裹，设置 `shadow="never"`
- 内部使用 `el-form` 组件，设置 `inline` 属性实现行内布局
- 筛选条件布局原则：
  - 当筛选条件较少（3-4项）时，保持在一行内展示
  - 当筛选条件较多时，应采用多行布局，每行放置3-4个筛选项
  - 无论单行还是多行，查询和重置按钮始终位于最后一行的最右侧

### 3.2 筛选区样式

- **背景色**：#FFFFFF
- **边框**：无边框或极细边框（1px solid #EBEEF5）
- **内部间距**：16px 20px
- **区域底部**：16px 的外边距，与表格区分隔

### 3.3 筛选区域布局示例

```vue
<template>
  <el-card shadow="never" class="filter-container">
    <el-form :model="filterForm" inline class="multi-line-filter-form">
      <!-- 第一行筛选项 -->
      <div class="filter-line">
        <el-form-item label="订单号：" prop="orderNo">
          <el-input
            v-model="filterForm.orderNo"
            placeholder="请输入订单号"
            clearable
            style="width: 220px"
          />
        </el-form-item>
        
        <el-form-item label="商户：" prop="merchantId">
          <el-select
            v-model="filterForm.merchantId"
            placeholder="请选择商户"
            clearable
            style="width: 168px"
          >
            <el-option
              v-for="item in merchantOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>
      </div>
      
      <!-- 第二行筛选项 -->
      <div class="filter-line">
        <el-form-item label="订单状态：" prop="status">
          <el-select
            v-model="filterForm.status"
            placeholder="请选择状态"
            clearable
            style="width: 168px"
          >
            <el-option
              v-for="item in statusOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>
        
        <!-- 操作按钮组，靠右对齐 -->
        <div class="filter-buttons">
          <el-button type="primary" @click="handleFilter">查询</el-button>
          <el-button plain @click="resetFilter">重置</el-button>
        </div>
      </div>
    </el-form>
  </el-card>
</template>

<style scoped>
.filter-container {
  margin-bottom: 16px;
}

.multi-line-filter-form .filter-line {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}

.multi-line-filter-form .filter-line:last-child {
  margin-bottom: 0;
  justify-content: flex-start;
}

.multi-line-filter-form .el-form-item {
  margin-bottom: 0;
  margin-right: 20px;
}

.filter-buttons {
  margin-left: auto;
  display: flex;
  align-items: center;
}

.filter-buttons .el-button + .el-button {
  margin-left: 12px;
}
</style>
```

## 四、表格区域规范

### 4.1 表格基础结构

- 使用 `el-card` 组件包裹表格，设置 `shadow="never"`
- 表格采用 `el-table` 组件，搭配 `el-table-column` 定义列

### 4.2 表格样式

- **边框**：推荐使用 `border` 属性，增强可读性
- **斑马纹**：建议启用 `stripe` 属性，提高行数据区分度
- **行高**：默认 54px，紧凑型可设置为 48px
- **表头**：固定表头 `header-fixed`，增强滚动时的可用性

### 4.3 表格工具栏布局

- **位置**：工具栏位于表格上方，与表格保持16px的间距
- **布局结构**：采用左右两端对齐的布局方式
  - 左侧：放置主要操作按钮，如"新增"、"批量删除"、"导出"等
  - 右侧：放置辅助功能按钮，如"刷新"、"密度"、"列设置"等

### 4.4 表格操作列布局

- **位置**：操作列通常固定在表格最右侧（`fixed="right"`）
- **宽度**：根据按钮数量设置合适宽度，避免挤压或过宽
- **按钮类型**：统一使用 `link` 类型按钮，减少视觉干扰
- **按钮排序**：查看 > 编辑 > 审核/处理 > 其他 > 删除（危险操作始终放在最后）
- **图标使用**：建议操作按钮添加图标，提高辨识度和美观性

**操作列按钮样式规范：**

1. **纯图标按钮**：适用于操作按钮较多，空间有限的场景
   - 使用 Element Plus 自带图标
   - 必须添加 tooltip 提示文字
   - 图标大小保持一致

2. **图标+文字按钮**：适用于重要操作，需要突出显示的场景
   - 图标放置在文字左侧
   - 图标与文字之间保持适当间距
   - 使用 `link` 类型，搭配适当的类型颜色

**常用操作图标选择参考：**
- 查看：`View` 或 `InfoFilled`
- 编辑：`Edit` 或 `EditPen`
- 删除：`Delete` 或 `Close`
- 审核：`Check` 或 `CircleCheck`
- 导出：`Download` 或 `DocumentCopy`

**操作按钮样式示例：**

```vue
<template>
  <el-table-column label="操作" width="150" fixed="right">
    <template #default="{ row }">
      <!-- 方式一：图标+文字按钮 -->
      <el-button link type="primary" :icon="Edit" @click="handleEdit(row)">编辑</el-button>
      <el-popconfirm title="确认删除该条数据?" @confirm="handleDelete(row)">
        <template #reference>
          <el-button link type="danger" :icon="Delete">删除</el-button>
        </template>
      </el-popconfirm>
      
      <!-- 方式二：纯图标按钮(紧凑型) -->
      <!--
      <el-tooltip content="编辑">
        <el-button link type="primary" :icon="Edit" @click="handleEdit(row)" />
      </el-tooltip>
      <el-popconfirm title="确认删除该条数据?" @confirm="handleDelete(row)">
        <template #reference>
          <el-tooltip content="删除">
            <el-button link type="danger" :icon="Delete" />
          </el-tooltip>
        </template>
      </el-popconfirm>
      -->
    </template>
  </el-table-column>
</template>

<script setup>
import { Edit, Delete } from '@element-plus/icons-vue'
</script>
```

**表格操作列完整示例（含完整引入图标）：**

```vue
<template>
  <el-table 
    :data="tableData" 
    border 
    stripe
    v-loading="tableLoading">
    <el-table-column prop="date" label="日期" />
    <el-table-column prop="name" label="名称" />
    <el-table-column prop="status" label="状态">
      <template #default="{ row }">
        <el-tag :type="row.status === '1' ? 'success' : 'danger'">
          {{ row.status === '1' ? '启用' : '禁用' }}
        </el-tag>
      </template>
    </el-table-column>
    
    <!-- 操作列 -->
    <el-table-column label="操作" width="180" fixed="right">
      <template #default="{ row }">
        <el-button link :icon="View" @click="handleView(row)">查看</el-button>
        <el-button link type="primary" :icon="Edit" @click="handleEdit(row)">编辑</el-button>
        <el-popconfirm title="确认删除?" @confirm="handleDelete(row)">
          <template #reference>
            <el-button link type="danger" :icon="Delete">删除</el-button>
          </template>
        </el-popconfirm>
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
import { View, Edit, Delete } from '@element-plus/icons-vue'

const handleView = (row) => {
  // 查看逻辑
}

const handleEdit = (row) => {
  // 编辑逻辑
}

const handleDelete = (row) => {
  // 删除逻辑
}
</script>
```

**样式补充说明：**
- 图标按钮样式可通过全局CSS调整，确保统一
- 图标与文字间距可通过CSS微调，默认为适当间距
- 同一行操作按钮应保持高度一致，避免视觉混乱

```css
/* 可选的全局样式调整 */
.el-button [class*="el-icon"] + span {
  margin-left: 4px; /* 调整图标和文字的间距 */
}

.el-table .el-button.el-button--small {
  padding: 6px 8px; /* 适当调整小尺寸按钮的内边距 */
}
```

### 4.5 分页器布局

- **位置**：分页器统一放置在表格下方，与表格保持16px间距
- **对齐方式**：完整分页靠右对齐 `justify-content: flex-end`
- **样式配置**：完整分页：`layout="total, sizes, prev, pager, next, jumper"`
- **每页条数**：默认提供 `[10, 20, 50, 100]` 选项

## 五、样式级规范

### 5.1 间距系统 (Spacing)

| 场景            | 值    | 实现方式             |
|-----------------|-------|----------------------|
| 页面外边距      | 20px  | `.container { margin: 20px }` |
| 区块间距        | 16px  | `.section { margin-bottom: 16px }` |
| 表单项水平间距   | 20px  | `.el-form-item { margin-right: 20px }` |
| 按钮组内部间距   | 12px  | `.el-button + .el-button { margin-left: 12px }` |
| 卡片内边距      | 20px  | `.el-card__body { padding: 20px }` |
| 表格单元格内边距 | 12px  | `.el-table .cell { padding: 0 12px }` |

### 5.2 响应式规则 (Responsive)

**断点处理：**
```css
/* 小屏幕筛选区处理 */
@media (max-width: 768px) {
  .filter-line {
    flex-wrap: wrap;
    > * { 
      margin-bottom: 12px;
      width: 100% !important; 
    }
  }
  
  .el-form-item {
    margin-right: 0;
  }
}

/* 中屏幕布局调整 */
@media (max-width: 992px) {
  .layout-container {
    flex-direction: column;
  }
  
  .el-aside {
    width: 100% !important;
    height: auto;
  }
}
```

**栅格策略：**
- ≥1200px: 每行 4-6 个筛选项
- 992px~1199px: 每行 3-4 个
- 768px~991px: 每行 2-3 个
- <768px: 每行 1 个（堆叠显示）

### 5.3 颜色系统 (Colors)

| 场景            | 颜色值        | CSS 变量                  |
|-----------------|--------------|--------------------------|
| 主色调          | #409EFF      | `--el-color-primary`     |
| 成功色          | #67C23A      | `--el-color-success`     |
| 警告色          | #E6A23C      | `--el-color-warning`     |
| 危险色          | #F56C6C      | `--el-color-danger`      |
| 边框色          | #EBEEF5      | `--el-border-color`      |
| 背景色          | #F5F7FA      | `--el-background-color`  |
| 文本主色        | #303133      | `--el-text-color-primary`|

## 六、交互级规范

### 6.1 表单验证 (Validation)

**规则：**
- 必填项必须添加 `required` 属性
- 错误提示统一显示在表单项下方
- 异步验证需使用 `async-validator`

**代码模式：**
```vue
<el-form-item 
  label="用户名" 
  prop="username"
  :rules="[
    { required: true, message: '请输入用户名', trigger: 'blur' },
    { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
  ]"
>
  <el-input v-model="form.username" />
</el-form-item>
```

### 6.2 加载状态 (Loading)

**规则：**
- 页面级加载使用 `v-loading` 指令
- 按钮级加载使用 `:loading="isLoading"` 属性
- 表格加载禁用选择功能

**实现示例：**
```vue
<!-- 页面级加载 -->
<div class="container" v-loading="pageLoading" element-loading-text="加载中...">
  <!-- 页面内容 -->
</div>

<!-- 表格加载 -->
<el-table v-loading="tableLoading">
  <!-- 表格内容 -->
</el-table>

<!-- 按钮加载 -->
<el-button type="primary" :loading="submitLoading" @click="handleSubmit">提交</el-button>
```

### 6.3 消息通知 (Notification)

**规则：**
- 操作成功使用 `ElMessage.success`
- 操作失败使用 `ElMessage.error`
- 需要用户确认的操作使用 `ElMessageBox.confirm`

**实现示例：**
```js
// 成功提示
const handleSuccess = () => {
  ElMessage.success('保存成功');
};

// 错误提示
const handleError = (error) => {
  ElMessage.error(error.message || '操作失败');
};

// 确认操作
const handleConfirmDelete = (id) => {
  ElMessageBox.confirm('此操作将永久删除该数据, 是否继续?', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    deleteItem(id);
    ElMessage.success('删除成功');
  }).catch(() => {
    ElMessage.info('已取消删除');
  });
};
```

## 七、安全规范

### 7.1 危险操作 (Dangerous Actions)

**约束：**
- 删除操作必须二次确认
- 批量操作需显示影响数量
- 敏感操作记录日志

**代码模板：**
```vue
<el-popconfirm 
  title="确认删除这 3 项数据？"
  @confirm="handleBatchDelete"
>
  <template #reference>
    <el-button type="danger">批量删除</el-button>
  </template>
</el-popconfirm>
```

### 7.2 权限控制 (Permissions)

**规则：**
- 按钮级权限使用 `v-permission` 指令
- 路由级权限在路由守卫中控制
- 敏感数据需要加密传输

**实现示例：**
```vue
<el-button 
  v-permission="['user:add']" 
  type="primary" 
  @click="handleAdd"
>
  新增用户
</el-button>
```

## 八、代码模式检测

### 8.1 必须包含项

| 组件类型      | 必须包含                          |
|--------------|-----------------------------------|
| 筛选区       | el-card + el-form + 查询/重置按钮 |
| 表格         | 分页组件 + 操作列 + 加载状态      |
| 表单弹窗     | el-dialog + 取消/确认按钮组       |
| 详情页       | 返回按钮 + 信息展示 + 操作区域    |

### 8.2 禁止使用项

- 禁止使用 `!important` 强制覆盖样式
- 禁止使用行内样式 (style attribute)
- 禁止使用已废弃的组件 API
- 禁止在模板中写复杂逻辑，应提取为方法
- 禁止直接修改 props 数据

## 九、变量命名规则

| 场景           | 命名模式              | 示例                       |
|----------------|----------------------|----------------------------|
| 筛选表单       | filterForm           | filterForm.orderNo         |
| 表格数据       | tableData            | tableData                  |
| 分页参数       | pagination           | pagination.pageSize        |
| 加载状态       | [action]Loading      | tableLoading               |
| 下拉选项       | [module]Options      | statusOptions              |
| 处理函数       | handle[Action]       | handleEdit                 |
| 表单对象       | formData             | formData                   |
| 弹窗状态       | [module]DialogVisible| editDialogVisible          |

## 十、常用布局模板

### 10.1 标准列表页面模板

```vue
<template>
  <div class="page-container">
    <!-- 筛选区 -->
    <el-card class="filter-container" shadow="never">
      <el-form :model="filterForm" inline>
        <!-- 筛选表单内容 -->
        <el-form-item label="关键词">
          <el-input v-model="filterForm.keyword" placeholder="请输入关键词" clearable />
        </el-form-item>
        <el-form-item label="状态">
          <el-select v-model="filterForm.status" placeholder="请选择状态" clearable>
            <el-option label="启用" value="1" />
            <el-option label="禁用" value="0" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="handleSearch">查询</el-button>
          <el-button @click="resetFilter">重置</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    
    <!-- 表格区 -->
    <el-card class="table-container" shadow="never">
      <div class="table-toolbar">
        <div class="left">
          <el-button type="primary" @click="handleAdd">新增</el-button>
        </div>
        <div class="right">
          <el-button @click="refreshTable">刷新</el-button>
        </div>
      </div>
      
      <el-table
        v-loading="tableLoading"
        :data="tableData"
        border
        stripe
        @selection-change="handleSelectionChange"
      >
        <el-table-column type="selection" width="55" />
        <el-table-column prop="name" label="名称" />
        <el-table-column prop="status" label="状态">
          <template #default="{ row }">
            <el-tag :type="row.status === '1' ? 'success' : 'danger'">
              {{ row.status === '1' ? '启用' : '禁用' }}
            </el-tag>
          </template>
        </el-table-column>
        <el-table-column prop="createTime" label="创建时间" />
        <el-table-column label="操作" width="180" fixed="right">
          <template #default="{ row }">
            <el-button link :icon="View" @click="handleView(row)">查看</el-button>
            <el-button link type="primary" :icon="Edit" @click="handleEdit(row)">编辑</el-button>
            <el-popconfirm title="确认删除?" @confirm="handleDelete(row)">
              <template #reference>
                <el-button link type="danger" :icon="Delete">删除</el-button>
              </template>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
      
      <div class="pagination-container">
        <el-pagination
          v-model:current-page="pagination.currentPage"
          v-model:page-size="pagination.pageSize"
          :page-sizes="[10, 20, 50, 100]"
          layout="total, sizes, prev, pager, next, jumper"
          :total="pagination.total"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </div>
    </el-card>
    
    <!-- 弹窗表单 -->
    <el-dialog
      :title="formTitle"
      v-model="dialogVisible"
      width="500px"
      destroy-on-close
    >
      <el-form
        ref="formRef"
        :model="form"
        :rules="rules"
        label-width="100px"
      >
        <el-form-item label="名称" prop="name" required>
          <el-input v-model="form.name" placeholder="请输入名称" />
        </el-form-item>
        <el-form-item label="状态" prop="status">
          <el-radio-group v-model="form.status">
            <el-radio label="1">启用</el-radio>
            <el-radio label="0">禁用</el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="submitForm" :loading="submitLoading">确认</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { View, Edit, Delete } from '@element-plus/icons-vue'

const handleView = (row) => {
  // 查看操作
}

const handleEdit = (row) => {
  // 编辑操作
}

const handleDelete = (row) => {
  // 删除操作
}

const handleSearch = () => {
  // 查询操作
}

const resetFilter = () => {
  // 重置筛选条件
}

const handleAdd = () => {
  // 新增操作
}

const refreshTable = () => {
  // 刷新表格
}

const handleSelectionChange = (selection) => {
  // 表格选择项变化
}

const handleSizeChange = (size) => {
  // 每页条数变化
}

const handleCurrentChange = (page) => {
  // 页码变化
}

const submitForm = () => {
  // 表单提交
}
</script>

<style scoped>
.page-container {
  padding: 20px;
}

.filter-container {
  margin-bottom: 16px;
}

.table-toolbar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
}

.pagination-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 16px;
}
</style>
```

## 十一、使用指南

开发者可复制以下模板请求 AI 生成代码：

```markdown
请基于 Element Plus 后台管理系统规范生成用户管理页面：
1. 布局：三段式布局，侧边栏210px
2. 筛选区：包含用户名搜索、状态筛选、日期范围筛选
3. 表格：显示用户名、角色、状态、最后登录时间、创建时间、操作列
4. 交互：删除操作需要二次确认，批量删除功能
5. 样式：符合间距系统规范，启用响应式布局
6. 功能：包含新增/编辑用户的表单弹窗，字段验证规则完善
``` 