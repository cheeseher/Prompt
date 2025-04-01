
# 后台管理系统 - 设计规范文档

本文档定义了基于 Element Plus 组件库的页面设计规范，确保系统界面视觉一致、交互流畅，并符合现代化后台管理系统的设计理念。

## 一、整体布局规范

### 1.1 页面结构

系统采用经典的"头部-侧边栏-内容区"三段式布局：

- **头部区域**：高度固定为 60px，包含 logo、系统名称、全局搜索、用户信息等
- **侧边栏**：固定宽度 210px，显示菜单导航
- **内容区**：自适应宽度，包含页面标题、筛选区、内容展示区等

### 1.2 栅格系统

采用 Element Plus 的 24 栏栅格系统，根据内容区宽度灵活布局：

- **大屏（≥1200px）**：筛选项每行 4-6 个字段
- **中屏（≥992px）**：筛选项每行 3-4 个字段
- **小屏（≥768px）**：筛选项每行 2-3 个字段

### 1.3 间距与留白

- **页面外边距**：内容区域与容器边缘间距 20px
- **区块间距**：相邻内容区块之间保持 16px-24px 间隔
- **元素间距**：同一区块内部元素间距 12px
- **内部填充**：内容容器内部填充 16px-20px

## 二、筛选区域规范

### 2.1 筛选区结构

- 筛选区使用 `el-card` 组件包裹，设置 `shadow="never"`
- 内部使用 `el-form` 组件，设置 `inline` 属性实现行内布局
- 表单项使用 `el-form-item` 组件，每项设置合理的 `label-width`
- 筛选条件布局原则：
  - 当筛选条件较少（3-4项）时，保持在一行内展示
  - 当筛选条件较多时，应采用多行布局，每行放置3-4个筛选项
  - 无论单行还是多行，查询和重置按钮始终位于最后一行的最右侧

### 2.2 筛选区样式

- **背景色**：#FFFFFF
- **边框**：无边框或极细边框（1px solid #EBEEF5）
- **内部间距**：16px 20px
- **区域底部**：16px 的外边距，与表格区分隔
- **行间距**：
  - 单行布局时，垂直间距设置为 0
  - 多行布局时，行与行之间的垂直间距设置为 16px，使筛选项分组清晰可辨
- **表单项间距**：相邻表单项之间的水平间距保持 16px 或 20px

### 2.3 筛选项组件规范

- **输入框**：统一高度 32px，宽度根据内容自适应，最小 168px
- **下拉菜单**：统一宽度 168px，搭配 `el-select` 组件使用
- **日期选择器**：使用 `el-date-picker` 组件，设置合理的默认值和快捷选项
- **按钮组**：操作按钮统一靠右对齐，位于筛选区最右侧
  - 主按钮（如"查询"）使用 `type="primary"`
  - 次按钮（如"重置"）使用 `plain` 属性
  - 按钮组与筛选项之间保持一定间距，确保视觉分离

### 2.4 按钮位置与对齐

- **对齐方式**：查询和重置按钮始终靠右对齐
  - 单行布局时，使用 CSS `margin-left: auto` 实现
  - 多行布局时，在最后一行设置 `justify-content: flex-end`
- **垂直对齐**：按钮与筛选表单项垂直居中对齐
- **组合规则**：
  - "查询"按钮始终位于最右侧第一个位置
  - "重置"按钮紧随"查询"按钮之后
  - 其他操作按钮（如"导出"）按照重要性依次排列
- **快捷键支持**：查询按钮可绑定 Enter 键，支持快速提交

### 2.5 筛选区布局示例

```html
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
      
      <el-form-item label="支付方式：" prop="paymentType">
        <el-select
          v-model="filterForm.paymentType"
          placeholder="请选择支付方式"
          clearable
          style="width: 168px"
        >
          <el-option
            v-for="item in paymentOptions"
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
      
      <el-form-item label="订单时间：" prop="dateRange">
        <el-date-picker
          v-model="filterForm.dateRange"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          value-format="YYYY-MM-DD"
          :shortcuts="dateShortcuts"
        />
      </el-form-item>
      
      <!-- 操作按钮组，靠右对齐 -->
      <div class="filter-buttons">
        <el-button type="primary" @click="handleFilter">查询</el-button>
        <el-button plain @click="resetFilter">重置</el-button>
        <el-button plain @click="handleExport">导出</el-button>
      </div>
    </div>
  </el-form>
</el-card>
```

```css
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
```

### 2.6 筛选区域布局技巧

- **多行筛选布局**：
  - 使用 CSS Grid 或 Flex 布局实现规整的多行效果
  - 每行筛选项保持在3-4个，避免过于拥挤
  - 最后一行可以少于3-4个，但按钮组始终靠右对齐
  - 示例实现：
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
        
        <el-form-item label="支付方式：" prop="paymentType">
          <el-select
            v-model="filterForm.paymentType"
            placeholder="请选择支付方式"
            clearable
            style="width: 168px"
          >
            <el-option
              v-for="item in paymentOptions"
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
        
        <el-form-item label="订单时间：" prop="dateRange">
          <el-date-picker
            v-model="filterForm.dateRange"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            value-format="YYYY-MM-DD"
            :shortcuts="dateShortcuts"
          />
        </el-form-item>
        
        <!-- 操作按钮组，靠右对齐 -->
        <div class="filter-buttons">
          <el-button type="primary" @click="handleFilter">查询</el-button>
          <el-button plain @click="resetFilter">重置</el-button>
          <el-button plain @click="handleExport">导出</el-button>
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
- **筛选项收起与展开**：当筛选项超过7-8个时，考虑实现收起和展开功能
  - 默认显示最常用的3-4个筛选条件和操作按钮
  - 提供"展开/收起"切换按钮，点击后显示或隐藏额外的筛选条件
  - 按钮组始终保持可见且靠右对齐
- **响应式处理**：
  - 在小屏幕设备上，自动切换为多行布局
  - 对于非常窄的屏幕，考虑使用替代的抽屉式筛选面板
- **空间分配**：
  - 筛选项均匀分布，保持视觉平衡
  - 标签宽度（`label-width`）应统一，提高对齐美观度
  - 针对较长的筛选条件，可适当增加宽度
- **交互优化**：
  - 输入框提供清空按钮
  - 下拉选择器支持清空选项
  - 日期选择器提供常用快捷选项

## 三、表格区域规范

### 3.1 表格基础结构

- 使用 `el-card` 组件包裹表格，设置 `shadow="never"`
- 表格采用 `el-table` 组件，搭配 `el-table-column` 定义列

### 3.2 表格样式

- **边框**：推荐使用 `border` 属性，增强可读性
- **斑马纹**：建议启用 `stripe` 属性，提高行数据区分度
- **行高**：默认 54px，紧凑型可设置为 48px
- **表头**：固定表头 `header-fixed`，增强滚动时的可用性
- **列宽**：根据内容合理分配，文本型列不低于 120px，操作列根据按钮数量设置

### 3.3 表格功能配置

- **分页**：表格下方统一使用 `el-pagination` 组件，设置合理的每页条数选项
- **排序**：允许点击表头排序的列，设置 `sortable` 属性
- **筛选**：需要列筛选的场景，配置 `filters` 和 `filter-method`
- **固定列**：首列（如序号、选择框）和操作列建议设置 `fixed` 属性
- **工具栏**：表格上方可放置批量操作按钮、刷新、密度调整等工具

### 3.4 表格工具栏布局

- **位置**：工具栏位于表格上方，与表格保持16px的间距
- **布局结构**：采用左右两端对齐的布局方式
  - 左侧：放置主要操作按钮，如"新增"、"批量删除"、"导出"等
  - 右侧：放置辅助功能按钮，如"刷新"、"密度"、"列设置"等
- **按钮排序**：
  - 主要按钮排序：新增 > 批量删除 > 批量导出 > 打印 > 其他操作
  - "新增"按钮使用 `type="primary"`，其他按钮使用 `plain` 属性
  - 危险操作（如"批量删除"）可使用 `type="danger" plain` 样式
- **批量操作按钮**：只在选中表格行时启用，未选中时禁用（`disabled`）
- **图标使用**：主要操作按钮建议使用文字+图标组合，提高识别度
  - 新增：`:icon="Plus"`
  - 删除：`:icon="Delete"`
  - 导出：`:icon="Download"`
  - 打印：`:icon="Printer"`
  - 刷新：`:icon="Refresh"`

### 3.5 表格操作列布局

- **位置**：操作列通常固定在表格最右侧（`fixed="right"`）
- **宽度**：根据按钮数量设置合适宽度，避免挤压或过宽
- **按钮类型**：统一使用 `link` 类型按钮，减少视觉干扰
- **按钮排序**：
  - 查看 > 编辑 > 审核/处理 > 其他 > 删除（危险操作始终放在最后）
- **图标使用**：
  - 为保持表格视觉简洁，操作按钮可以只使用文字，不使用图标
  - 如需使用图标，可采用以下方式之一：
    1. 所有操作按钮都使用文字+图标组合（`:icon="View"` + 文字）
    2. 操作非常多时，可只使用图标按钮搭配 `el-tooltip` 提示
  - 图标选择参考：
    - 查看：`View` 或 `InfoFilled`
    - 编辑：`Edit` 或 `EditPen`
    - 删除：`Delete` 或 `Remove`
    - 审核：`Check` 或 `CircleCheck`
- **权限控制**：根据行数据状态和用户权限动态显示或隐藏按钮
- **删除操作**：删除操作必须使用 `el-popconfirm` 进行二次确认

### 3.6 分页器布局

- **位置**：分页器统一放置在表格下方，与表格保持16px间距
- **对齐方式**：
  - 基础分页：居中对齐 `justify-content: center`
  - 完整分页：靠右对齐 `justify-content: flex-end`
- **样式配置**：
  - 基础分页：`layout="prev, pager, next, jumper"`
  - 完整分页：`layout="total, sizes, prev, pager, next, jumper"`
- **每页条数**：默认提供 `[10, 20, 50, 100]` 选项
- **移动端适配**：小屏幕下可简化为 `layout="prev, pager, next"`

### 3.7 表格布局示例（完整版）

```html
<el-card shadow="never">
  <!-- 表格工具栏 -->
  <div class="table-toolbar">
    <div class="left">
      <el-button type="primary" :icon="Plus" @click="handleAdd">新增</el-button>
      <el-button :icon="Delete" plain :disabled="!hasSelected" @click="handleBatchDelete">批量删除</el-button>
      <el-button :icon="Download" plain @click="handleExport">导出</el-button>
      <el-button :icon="Printer" plain @click="handlePrint">打印</el-button>
    </div>
    <div class="right">
      <el-tooltip content="刷新数据">
        <el-button :icon="Refresh" circle plain @click="refreshTable" />
      </el-tooltip>
      <el-tooltip content="密度">
        <el-dropdown trigger="click" @command="handleSizeChange">
          <el-button :icon="Grid" circle plain />
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item command="large">默认</el-dropdown-item>
              <el-dropdown-item command="default">中等</el-dropdown-item>
              <el-dropdown-item command="small">紧凑</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </el-tooltip>
      <el-tooltip content="列设置">
        <el-button :icon="SetUp" circle plain @click="handleColumnSetting" />
      </el-tooltip>
    </div>
  </div>
  
  <!-- 表格主体 -->
  <el-table
    v-loading="loading"
    :data="tableData"
    border
    stripe
    height="calc(100vh - 300px)"
    @selection-change="handleSelectionChange"
  >
    <!-- 选择列 -->
    <el-table-column
      type="selection"
      width="55"
      fixed="left"
    />
    
    <!-- 序号列 -->
    <el-table-column
      type="index"
      label="序号"
      width="80"
      fixed="left"
    />
    
    <!-- 数据列 -->
    <el-table-column
      prop="orderNo"
      label="订单号"
      min-width="180"
      show-overflow-tooltip
    />
    
    <el-table-column
      prop="status"
      label="状态"
      width="100"
    >
      <template #default="{ row }">
        <el-tag :type="getStatusType(row.status)">
          {{ getStatusText(row.status) }}
        </el-tag>
      </template>
    </el-table-column>
    
    <!-- 操作列 -->
    <el-table-column
      label="操作"
      width="180"
      fixed="right"
    >
      <template #default="{ row }">
        <!-- 方式一：纯文字按钮 -->
        <el-button link type="primary" @click="handleView(row)">查看</el-button>
        <el-button link type="primary" @click="handleEdit(row)" v-if="row.status === 'pending'">编辑</el-button>
        <el-button link type="success" @click="handleApprove(row)" v-if="row.status === 'pending'">审核</el-button>
        <el-popconfirm title="确认删除？" @confirm="handleDelete(row)">
          <template #reference>
            <el-button link type="danger">删除</el-button>
          </template>
        </el-popconfirm>
        
        <!-- 方式二：图标+文字按钮（二选一）
        <el-button link type="primary" :icon="View" @click="handleView(row)">查看</el-button>
        <el-button link type="primary" :icon="EditPen" @click="handleEdit(row)" v-if="row.status === 'pending'">编辑</el-button>
        <el-button link type="success" :icon="Check" @click="handleApprove(row)" v-if="row.status === 'pending'">审核</el-button>
        <el-popconfirm title="确认删除？" @confirm="handleDelete(row)">
          <template #reference>
            <el-button link type="danger" :icon="Delete">删除</el-button>
          </template>
        </el-popconfirm>
        -->
        
        <!-- 方式三：纯图标按钮+提示（二选一，适用于操作很多的场景）
        <el-tooltip content="查看详情">
          <el-button link type="primary" :icon="View" @click="handleView(row)" />
        </el-tooltip>
        <el-tooltip content="编辑" v-if="row.status === 'pending'">
          <el-button link type="primary" :icon="EditPen" @click="handleEdit(row)" />
        </el-tooltip>
        <el-tooltip content="审核" v-if="row.status === 'pending'">
          <el-button link type="success" :icon="Check" @click="handleApprove(row)" />
        </el-tooltip>
        <el-popconfirm title="确认删除？" @confirm="handleDelete(row)">
          <template #reference>
            <el-tooltip content="删除">
              <el-button link type="danger" :icon="Delete" />
            </el-tooltip>
          </template>
        </el-popconfirm>
        -->
      </template>
    </el-table-column>
  </el-table>
  
  <!-- 分页器 -->
  <div class="pagination-container">
    <el-pagination
      v-model:current-page="pageParams.currentPage"
      v-model:page-size="pageParams.pageSize"
      :page-sizes="[10, 20, 50, 100]"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
</el-card>
```

```css
.table-toolbar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
}

.table-toolbar .left .el-button {
  margin-right: 8px;
}

.table-toolbar .right .el-button {
  margin-left: 8px;
}

.pagination-container {
  display: flex;
  justify-content: flex-end; /* 或使用 center */
  margin-top: 16px;
}
```

## 四、表单区域规范

### 4.1 表单布局

- **行内表单**：适用于简单筛选条件，使用 `inline` 属性
- **对齐表单**：复杂数据录入，使用 `label-position="right"` 和固定的 `label-width`
- **分组表单**：使用 `el-card` 或 `el-divider` 进行逻辑分组

### 4.2 表单验证

- 必填项标记使用 `required` 属性，显示红色星号
- 使用 `rules` 属性配置验证规则，搭配 `validate` 方法进行表单验证
- 错误提示统一显示在表单项下方

### 4.3 表单组件选择

- **短文本**：使用 `el-input`，设置合适的 `placeholder`
- **长文本**：使用 `el-input` 的 `type="textarea"` 属性
- **选择项**：
  - 少量选项：使用 `el-radio` 或 `el-checkbox`
  - 多个选项：使用 `el-select` 搭配 `el-option`
  - 树形选择：使用 `el-cascader` 或 `el-tree-select`
- **日期时间**：根据精度需求选择 `el-date-picker` 的不同 `type`
- **数字输入**：使用 `el-input-number`，配置 `min`、`max` 和 `step`

### 4.4 对话框表单

- 使用 `el-dialog` 组件，设置合理的宽度（一般为 500px-800px）
- 标题简洁明确，关闭按钮便于发现
- 底部按钮对齐方式为右对齐，取消按钮在左，确认按钮在右
- 大型表单考虑使用 `el-tabs` 分组展示

## 五、色彩与视觉规范

### 5.1 色彩系统

主要遵循 Element Plus 默认色彩系统：

- **主色**：#409EFF（蓝色）
- **成功色**：#67C23A（绿色）
- **警告色**：#E6A23C（橙色）
- **危险色**：#F56C6C（红色）
- **信息色**：#909399（灰色）

### 5.2 文字规范

- **主要文字**：#303133，用于重要标题、内容
- **常规文字**：#606266，用于正文内容
- **次要文字**：#909399，用于辅助、描述文字
- **占位文字**：#C0C4CC，用于输入框占位符

### 5.3 图标使用

- 统一使用 Element Plus 线性图标（非填充样式）
- 图标与文字搭配时，保持垂直居中对齐
- 主导航图标保持统一风格，二级菜单不使用图标

### 5.4 阴影效果

- 卡片容器推荐使用 `shadow="hover"` 或 `shadow="never"`
- 悬浮菜单、对话框使用默认阴影效果
- 避免过多重叠阴影，保持界面简洁清爽

## 六、响应式设计

### 6.1 断点设置

遵循 Element Plus 的响应式断点：

- **xs**：<768px（超小屏幕）
- **sm**：≥768px（小屏幕）
- **md**：≥992px（中等屏幕）
- **lg**：≥1200px（大屏幕）
- **xl**：≥1920px（超大屏幕）

### 6.2 响应式调整策略

- **导航栏**：小屏幕下可折叠或使用抽屉式导航
- **表格**：小屏幕优先展示核心列，次要信息可折叠或隐藏
- **表单**：小屏幕下由行内布局转为垂直布局
- **筛选项**：小屏幕下减少每行显示的筛选条件数量

## 七、交互规范

### 7.1 加载状态

- 页面初始加载使用全屏加载效果
- 表格数据加载使用 `v-loading` 指令
- 按钮加载状态使用 `loading` 属性，防止重复提交

### 7.2 空状态处理

- 表格无数据时，显示 `el-empty` 组件
- 列表为空时，提供友好的提示和可能的操作建议

### 7.3 错误处理

- 表单验证错误，在相应字段下方显示错误提示
- 操作失败时，使用 `el-message` 提示具体原因
- 系统错误使用 `el-alert` 组件，提供重试或联系支持的选项

### 7.4 操作反馈

- 成功操作使用 `el-message` 的 `success` 类型
- 危险操作（如删除）使用 `el-message-box` 进行二次确认
- 批量操作提供进度反馈，可使用 `el-progress` 组件

## 八、实际案例

### 8.1 筛选区域示例

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
        
        <el-form-item label="支付方式：" prop="paymentType">
          <el-select
            v-model="filterForm.paymentType"
            placeholder="请选择支付方式"
            clearable
            style="width: 168px"
          >
            <el-option
              v-for="item in paymentOptions"
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
        
        <el-form-item label="订单时间：" prop="dateRange">
          <el-date-picker
            v-model="filterForm.dateRange"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            value-format="YYYY-MM-DD"
            :shortcuts="dateShortcuts"
          />
        </el-form-item>
        
        <!-- 操作按钮组，靠右对齐 -->
        <div class="filter-buttons">
          <el-button type="primary" @click="handleFilter">查询</el-button>
          <el-button plain @click="resetFilter">重置</el-button>
          <el-button plain @click="handleExport">导出</el-button>
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

### 8.2 表格区域示例

```vue
<template>
  <el-card shadow="never">
    <div class="table-toolbar">
      <div>
        <el-button type="primary" @click="handleAdd">新增订单</el-button>
        <el-button plain @click="handleBatchExport">批量导出</el-button>
      </div>
      <div>
        <el-tooltip content="刷新数据">
          <el-button :icon="Refresh" circle plain @click="refreshTable" />
        </el-tooltip>
      </div>
    </div>
    
    <el-table
      v-loading="loading"
      :data="tableData"
      border
      stripe
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" fixed="left" />
      <el-table-column type="index" label="序号" width="80" fixed="left" />
      <el-table-column prop="orderNo" label="订单号" min-width="180" show-overflow-tooltip />
      <el-table-column prop="merchantName" label="商户名称" min-width="120" show-overflow-tooltip />
      <el-table-column prop="amount" label="订单金额" width="120">
        <template #default="{ row }">
          {{ formatCurrency(row.amount) }}
        </template>
      </el-table-column>
      <el-table-column prop="paymentTypeName" label="支付方式" width="120" />
      <el-table-column prop="status" label="状态" width="100">
        <template #default="{ row }">
          <el-tag :type="getStatusType(row.status)">
            {{ getStatusText(row.status) }}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="createTime" label="创建时间" width="180" />
      <el-table-column label="操作" width="220" fixed="right">
        <template #default="{ row }">
          <el-button link type="primary" @click="handleView(row)">查看</el-button>
          <el-button link type="primary" @click="handleEdit(row)" v-if="row.status === 'pending'">编辑</el-button>
          <el-button link type="success" @click="handleApprove(row)" v-if="row.status === 'pending'">审核</el-button>
          <el-popconfirm title="确认删除此订单？" @confirm="handleDelete(row)">
            <template #reference>
              <el-button link type="danger">删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    
    <div class="pagination-container">
      <el-pagination
        v-model:current-page="pageParams.currentPage"
        v-model:page-size="pageParams.pageSize"
        :page-sizes="[10, 20, 50, 100]"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </el-card>
</template>

<style scoped>
.table-toolbar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
}

.pagination-container {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}
</style>
```

## 九、最佳实践

### 9.1 界面加载优化

- 使用 `v-loading` 指令控制加载状态，避免界面抖动
- 针对大数据量表格，考虑使用虚拟滚动
- 图片资源使用懒加载，减少首屏加载时间

### 9.2 表单处理技巧

- 复杂表单拆分为多个逻辑部分，使用步骤条或标签页组织
- 保存用户输入状态，防止意外刷新导致数据丢失
- 提供智能默认值，减少用户输入负担

### 9.3 页面适配建议

- 优先实现大屏幕布局，再进行响应式降级
- 关键操作按钮始终保持可见性
- 复杂表格在小屏幕下提供替代视图（如卡片式布局）

### 9.4 性能优化

- 合理使用 `v-if` 和 `v-show`，减少不必要的 DOM 操作
- 对于大数据列表，使用分页或虚拟滚动技术
- 搜索操作添加防抖处理，避免频繁请求
- 使用计算属性缓存复杂计算结果

## 十、注意事项

1. 所有组件遵循 Element Plus 最新版本的 API 规范
2. 页面设计保持简洁，避免过度设计和不必要的视觉干扰
3. 保持交互一致性，相同功能在不同页面使用相同的交互模式
4. 重视无障碍设计，确保键盘可操作性和屏幕阅读器兼容性
5. 代码遵循模块化原则，确保维护性和可扩展性 