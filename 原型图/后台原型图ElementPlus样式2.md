# Element Plus 后台管理系统设计规范

## Ⅰ. 组件规范

### 1. 表格组件规范

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

### 2. 表单组件规范

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

## Ⅱ. 样式级规范

### 1. 间距系统 (Spacing)

| 场景            | 值    | 实现方式             |
|-----------------|-------|----------------------|
| 页面外边距      | 20px  | `.container { margin: 20px }` |
| 区块间距        | 16px  | `.section { margin-bottom: 16px }` |
| 表单项水平间距   | 20px  | `.el-form-item { margin-right: 20px }` |
| 按钮组内部间距   | 12px  | `.el-button + .el-button { margin-left: 12px }` |
| 卡片内边距      | 20px  | `.el-card__body { padding: 20px }` |
| 表格单元格内边距 | 12px  | `.el-table .cell { padding: 0 12px }` |

### 2. 响应式规则 (Responsive)

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

### 3. 颜色系统 (Colors)

| 场景            | 颜色值        | CSS 变量                  |
|-----------------|--------------|--------------------------|
| 主色调          | #409EFF      | `--el-color-primary`     |
| 成功色          | #67C23A      | `--el-color-success`     |
| 警告色          | #E6A23C      | `--el-color-warning`     |
| 危险色          | #F56C6C      | `--el-color-danger`      |
| 边框色          | #EBEEF5      | `--el-border-color`      |
| 背景色          | #F5F7FA      | `--el-background-color`  |
| 文本主色        | #303133      | `--el-text-color-primary`|

## Ⅲ. 交互级规范

### 1. 表单验证 (Validation)

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

### 2. 加载状态 (Loading)

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

### 3. 消息通知 (Notification)

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

## Ⅳ. 安全规范

### 1. 危险操作 (Dangerous Actions)

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

### 2. 权限控制 (Permissions)

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

## Ⅴ. 代码模式检测

### 1. 必须包含项

| 组件类型      | 必须包含                          |
|--------------|-----------------------------------|
| 筛选区       | el-card + el-form + 查询/重置按钮 |
| 表格         | 分页组件 + 操作列 + 加载状态      |
| 表单弹窗     | el-dialog + 取消/确认按钮组       |
| 详情页       | 返回按钮 + 信息展示 + 操作区域    |

### 2. 禁止使用项

- 禁止使用 `!important` 强制覆盖样式
- 禁止使用行内样式 (style attribute)
- 禁止使用已废弃的组件 API
- 禁止在模板中写复杂逻辑，应提取为方法
- 禁止直接修改 props 数据

## Ⅵ. 术语对照表

| 设计术语      | 代码实现               | 对应属性/组件           |
|--------------|------------------------|------------------------|
| 斑马纹表格    | el-table + stripe      | stripe                 |
| 行内表单      | el-form + inline       | inline                 |
| 紧凑模式      | el-table + size="small"| size="small"           |
| 轻量级筛选    | el-form + autoWidth    | label-width="auto"     |
| 动态表单      | el-form + schema       | FormRender 组件        |
| 卡片容器      | el-card                | el-card                |
| 抽屉表单      | el-drawer              | el-drawer              |
| 步骤表单      | el-steps + el-form     | el-steps               |

## Ⅶ. 变量命名规则

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

## Ⅷ. 常用布局模板

### 1. 标准列表页面

```vue
<template>
  <div class="page-container">
    <!-- 筛选区 -->
    <el-card class="filter-container">
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
    <el-card class="table-container">
      <div class="table-toolbar">
        <div class="left">
          <el-button type="primary" @click="handleAdd">新增</el-button>
        </div>
        <div class="right">
          <el-button @click="refreshTable">刷新</el-button>
        </div>
      </div>
      
      <!-- 表格组件 -->
      <!-- 分页组件 -->
    </el-card>
    
    <!-- 弹窗表单 -->
    <el-dialog
      :title="formTitle"
      v-model="dialogVisible"
      width="500px"
      destroy-on-close
    >
      <!-- 表单内容 -->
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="submitForm" :loading="submitLoading">确认</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>
```

> **提示使用：**  
> 开发者可复制以下模板请求 AI 生成代码：  
> ```markdown  
> 请基于以下规则生成用户管理页面：  
> 1. 布局：三段式布局，侧边栏210px  
> 2. 筛选区：包含用户名搜索、状态筛选、日期范围筛选  
> 3. 表格：显示用户名、角色、状态、最后登录时间、创建时间、操作列  
> 4. 交互：删除操作需要二次确认，批量删除功能  
> 5. 样式：符合间距系统规范，启用响应式布局  
> 6. 功能：包含新增/编辑用户的表单弹窗，字段验证规则完善  
> ```
