以下是基于场景的数据库表设计方案及其表之间的关系：

### 数据库表设计

#### 1. `products` 表：存储商品的基础信息。
- **表名**：`products`
- **关键字段**：
    - `id` (主键) - 商品 ID
    - `name` - 商品名称
    - `base_price` - 基础价格（不含规格价格）
    - `description` - 商品描述
    - `created_at` - 创建时间
    - `updated_at` - 更新时间

#### 2. `attributes` 表：存储商品的可选规格属性（如颜色、尺寸、材质等）。
- **表名**：`attributes`
- **关键字段**：
    - `id` (主键) - 规格属性 ID
    - `name` - 属性名称（如颜色、尺寸、材质）
    - `created_at` - 创建时间
    - `updated_at` - 更新时间

#### 3. `attribute_values` 表：存储具体的规格值（如红、黄、蓝）。
- **表名**：`attribute_values`
- **关键字段**：
    - `id` (主键) - 规格值 ID
    - `attribute_id` (外键) - 所属规格属性 ID，关联 `attributes.id`
    - `value` - 具体的规格值（如红、黄、蓝）
    - `created_at` - 创建时间
    - `updated_at` - 更新时间

#### 4. `product_attributes` 表：存储商品的所有规格选项。
- **表名**：`product_attributes`
- **关键字段**：
    - `id` (主键) - 记录 ID
    - `product_id` (外键) - 商品 ID，关联 `products.id`
    - `attribute_value_id` (外键) - 规格值 ID，关联 `attribute_values.id`
    - `created_at` - 创建时间
    - `updated_at` - 更新时间

#### 5. `product_prices` 表：存储商品在不同规格组合下的价格。
- **表名**：`product_prices`
- **关键字段**：
    - `id` (主键) - 记录 ID
    - `product_id` (外键) - 商品 ID，关联 `products.id`
    - `attribute_value_ids` - 规格值 ID 的组合（用逗号分隔，如 `1,4,7`）
    - `price` - 该规格组合对应的价格
    - `created_at` - 创建时间
    - `updated_at` - 更新时间

### 表之间的关系

1. `products` 与 `attributes` 是多对多关系，通过 `product_attributes` 关联。
2. `attributes` 与 `attribute_values` 是一对多关系。
3. `product_attributes` 是 `products` 和 `attribute_values` 的中间表，用于管理商品和规格值的关联。
4. `product_prices` 存储每种规格组合对应的价格，基于 `product_id` 和 `attribute_value_ids`。

### 示例数据

#### `products`
| id | name   | base_price | description | created_at        | updated_at        |
|----|--------|------------|-------------|-------------------|-------------------|
| 1  | 帽子   | 20         | 优质帽子    | 2024-12-04 10:00 | 2024-12-04 10:00 |

#### `attributes`
| id | name   | created_at        | updated_at        |
|----|--------|-------------------|-------------------|
| 1  | 颜色   | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 2  | 尺寸   | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 3  | 材质   | 2024-12-04 10:00 | 2024-12-04 10:00 |

#### `attribute_values`
| id | attribute_id | value | created_at        | updated_at        |
|----|--------------|-------|-------------------|-------------------|
| 1  | 1            | 红    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 2  | 1            | 黄    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 3  | 1            | 蓝    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 4  | 2            | M     | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 5  | 2            | L     | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 6  | 2            | XL    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 7  | 3            | 棉    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 8  | 3            | 纱    | 2024-12-04 10:00 | 2024-12-04 10:00 |

#### `product_prices`
| id | product_id | attribute_value_ids | price | created_at        | updated_at        |
|----|------------|---------------------|-------|-------------------|-------------------|
| 1  | 1          | 1,5,8              | 28    | 2024-12-04 10:00 | 2024-12-04 10:00 |
| 2  | 1          | 2,4,7              | 25    | 2024-12-04 10:00 | 2024-12-04 10:00 |

### 查询示例
用户选择了红（`attribute_value_id=1`）、L（`attribute_value_id=5`）、纱（`attribute_value_id=8`）后，可以查询：

```sql
SELECT price 
FROM product_prices 
WHERE product_id = 1 
  AND attribute_value_ids = '1,5,8';
```
结果返回价格 `28` 元。

Tip:
----
- 我的GitHub和gitee
- https://github.com/longdaihai
- https://gitee.com/haiyunyi_cn