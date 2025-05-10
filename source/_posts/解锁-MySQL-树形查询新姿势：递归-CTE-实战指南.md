title: 解锁 MySQL 树形查询新姿势：递归 CTE 实战指南
author: l1n6yun
date: 2025-05-10 10:10:34
tags:
---
## 一、MySQL 递归 CTE 基础概念

### 1.1 什么是递归 CTE？

递归公用表表达式（Recursive CTE）是 MySQL 8.0 引入的高级特性，通过`WITH RECURSIVE`语法定义，允许在 CTE 内部递归引用自身，专门用于处理具有层级关系的树形数据，如组织架构、分类目录、文件系统等。其核心思想是通过锚成员（初始查询）和递归成员（迭代查询）的结合，逐层扩展结果集，直至满足终止条件（递归成员返回空集）。

### 1.2 适用场景

**组织架构管理**：查询某个部门的所有上下级节点。

**分类目录遍历**：获取商品分类的全层级路径。

**树状结构分析**：查找节点的所有祖先或后代，替代传统自连接或存储过程的复杂逻辑。

## 二、递归 CTE 语法解析与执行逻辑

### 2.1 核心语法结构

```sql
WITH RECURSIVE cte_name (column_list) AS (
    -- 锚成员：定义递归初始条件，返回基础结果集
    initial_query    
    UNION ALL  -- 必须使用UNION ALL，确保递归过程高效合并结果
    -- 递归成员：引用CTE名称，定义迭代规则
    recursive_query   
)

-- 最终查询：基于CTE结果集进行筛选或关联
SELECT ... FROM cte_name;
```

### 2.2 执行流程详解

**锚成员执行**：生成初始结果集（R0），如指定节点的基础信息。

**递归迭代**：将上一次结果集（Ri）作为输入，通过`UNION ALL`合并新生成的结果集（Ri+1），直到递归成员返回空集。

**终止条件**：隐式终止于递归成员无数据返回，或显式通过条件（如`WHERE n < 100`）限制递归深度。

### 2.3 递归成员限制

禁止使用聚合函数（如`SUM`/`COUNT`）、`GROUP BY`、`ORDER BY`、`LIMIT`、`DISTINCT`（`UNION DISTINCT`除外）。

仅能引用 CTE 名称，不能嵌套子查询。

## 三、典型场景与实战案例

### 3.1 查询节点所有父节点（向上递归）

**场景**：从子节点出发，逐层查找所有上级节点（如员工查询其所有管理层级）。**表结构**：`club(id, name, pid)`，`pid`为父节点 ID，根节点`pid`为`NULL`。**SQL 示例**：

```sql
WITH RECURSIVE parent_path AS (
    -- 锚成员：初始节点（如id=5的子节点）
    SELECT id, name, pid FROM club WHERE id = 5    
    UNION ALL    
    -- 递归成员：通过pid关联，获取父节点
    SELECT p.id, p.name, p.pid FROM club p JOIN parent_path pp ON p.id = pp.pid    
)

SELECT * FROM parent_path;
```

**解析**：从 id=5 开始，每次递归通过`pid`找到父节点，直至无更高层级节点。

### 3.2 查询节点所有子节点（向下递归）

**场景**：从父节点出发，获取其所有直接及间接子节点（如部门主管查询下属团队）。**SQL 示例**：

```sql
WITH RECURSIVE child_path AS (
    SELECT id, name, pid FROM club WHERE id = 3  -- 锚成员：初始父节点
    UNION ALL    
    -- 递归成员：通过子节点pid关联父节点id
    SELECT c.id, c.name, c.pid FROM club c JOIN child_path cp ON c.pid = cp.id    
)

SELECT * FROM child_path;
```

**解析**：以 id=3 为起点，逐层匹配`pid=当前id`的子节点，实现无限层级遍历。

### 3.3 添加层级标识（Level 字段）

**场景**：在查询结果中显式节点层级，方便分页或排序（如目录树展示）。**SQL 示例**：

```sql
WITH RECURSIVE level_tree AS (
    SELECT id, name, pid, 1 AS level FROM club WHERE id = 1  -- 根节点层级为1
    UNION ALL    
    SELECT c.id, c.name, c.pid, ct.level + 1 AS level  -- 子节点层级=父层级+1
    FROM club c JOIN level_tree ct ON c.pid = ct.id    
)

-- 筛选层级>3的深层子节点

SELECT id, name, pid, level FROM level_tree WHERE level > 3;
```

**解析**：通过`level`字段量化层级深度，避免表设计时预存层级的冗余问题。

### 3.4 实战优化：业务层与 SQL 层解耦

**场景**：传统 Java/Python 代码中，递归遍历组织架构易导致性能瓶颈，改用递归 CTE 后可在数据库层高效完成。**MyBatis 映射示例**：

```sql
WITH RECURSIVE DeptTree AS (
  SELECT id_ FROM org_group WHERE id_ = #{deptId}  -- 锚成员：目标部门
  UNION ALL    
  SELECT og.id_ FROM org_group og JOIN DeptTree dt ON og.parent_id_ = dt.id_  -- 递归获取子部门
)
SELECT su.* FROM sys_user su JOIN DeptTree dt ON su.dept_id = dt.id_
```

**优势**：避免多次往返数据库，单条 SQL 完成层级查询，提升系统响应速度。

## 四、注意事项与最佳实践

### 4.1 MySQL 版本要求

仅支持 MySQL 8.0 及以上版本，低版本需使用存储过程或应用层递归实现。

### 4.2 避免死循环

**数据校验**：确保层级数据无环（如 A→B→A），否则递归会因无法终止报错（默认最大递归深度 1000，可通过`SET @@cte_max_recursion_depth = N`调整）。

**条件限制**：在递归成员中添加合理过滤条件（如`WHERE level < 50`），防止无限递归。

### 4.3 索引优化

为`id`和`pid`字段添加索引，提升递归过程中 JOIN 操作的效率，尤其对大规模层级数据至关重要。

### 4.4 结果去重

若数据存在重复关联，可在最终查询中使用`DISTINCT`去重，但需注意递归成员中禁止直接使用`DISTINCT`。

## 五、总结

递归 CTE 是 MySQL 处理树形数据的 “瑞士军刀”，通过简洁的语法将复杂的层级查询转化为结构化的递归过程，显著提升开发效率与查询性能。无论是组织架构、分类目录还是其他层级场景，掌握递归 CTE 的锚成员定义、递归规则设计及终止条件把控，都能让你在数据处理中游刃有余。建议在实际项目中结合索引优化与数据校验，充分发挥其在层级查询中的优势。

**动手实践**：尝试在示例表`club`中插入多级数据，分别编写查询根节点、叶节点及全路径的递归 CTE 语句，观察结果差异与执行效率。