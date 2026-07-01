---
name: refactor
description: 安全重构建议 — 只给方案不擅自执行，保留 docstring，行为不变。触发词：重构、refactor、优化结构、清理代码、改进设计
---

# Refactor — 安全重构建议

> 本 skill 是**重构方案生成器**，**不擅自执行**重构。

## 核心原则

1. **只给方案**：输出"应该怎么改"清单，**不**直接修改代码
2. **行为不变**：重构前后测试结果**必须一致**
3. **保留文档**：禁止删除已有 docstring / 注释
4. **TDD 验证**：每步重构后跑测试，确认仍为绿色
5. **小步前进**：每次只重构一个 smell，跑一次测试

## 重构工作流

```
第1步：验证基线 → 第2步：运行全部测试 → 第3步：执行重构 → 第4步：重新运行测试 → 第5步：复查
```

### 第1步：验证基线

```bash
git status
git stash  # 暂存未提交的变更（如有）

# 记录基线测试结果
pytest tests/ -v --tb=short > /tmp/baseline_tests.txt 2>&1
```

### 第2步：运行全部测试

```bash
pytest tests/ -v
```

- 必须 100% 通过
- 如有失败，先修复再重构

### 第3步：执行重构

常见重构操作：

| 重构类型 | 操作 | 安全校验 |
|---------|------|---------|
| 提取方法 | 将重复代码抽取为函数 | 确保签名兼容，所有调用点更新 |
| 重命名 | 改名变量/函数/类 | 全局搜索替换，检查外部引用 |
| 移动代码 | 模块间移动 | 更新 import 路径 |
| 简化条件 | 合并/化简 if-else | 覆盖所有分支的测试必须通过 |
| 提取常量 | 魔法数字→命名常量 | 值不变 |
| 引入参数 | 硬编码→参数化 | 默认值保持原行为 |
| 引入 dataclass | dict → @dataclass | 字段一一对应 |
| 用多态替代条件 | if/elif 链 → 类注册表 | 行为不变 |

约束：
- 每次只做一种重构
- 不在重构时同时添加新功能
- 不在重构时修改测试（除非测试因接口变更需要适配）
- 保留已有 docstring

### 第4步：重新运行测试

```bash
pytest tests/ -v
```

- 必须与基线结果一致（相同的通过数）
- 如出现新失败，说明重构引入了行为变更 → 回滚或修复

### 第5步：复查

```bash
git diff
git diff | grep -E "^\+.*def "  # 不应有新函数（除非是提取的私有方法）
```

复查清单：
- [ ] 所有测试通过，通过数与基线一致
- [ ] 没有新增公开 API
- [ ] 没有修改外部行为
- [ ] import 语句正确
- [ ] 文档注释仍然准确

## 通用重构手法

### 1. 提取函数（Extract Function）

**触发**: 函数 > 30 行 / 多个职责 / 注释解释了一段代码

**Before**:
```python
def process_data(msg):
    # 解析位置
    lat = msg.lat / 1e7
    lon = msg.lon / 1e7
    alt = msg.alt / 1000
    # 解析方向
    roll = math.degrees(msg.roll)
    pitch = math.degrees(msg.pitch)
    yaw = math.degrees(msg.yaw)
    # 写入缓存
    self._position = Position(lat=lat, lon=lon, alt=alt)
    self._attitude = Attitude(roll=roll, pitch=pitch, yaw=yaw)
```

**After**:
```python
def process_data(msg):
    """处理数据：解析位置和方向并更新缓存。"""
    self._position = self._parse_position(msg)
    self._attitude = self._parse_attitude(msg)

def _parse_position(self, msg) -> Position:
    return Position(
        lat=msg.lat / 1e7,
        lon=msg.lon / 1e7,
        alt=msg.alt / 1000,
    )

def _parse_attitude(self, msg) -> Attitude:
    return Attitude(
        roll=math.degrees(msg.roll),
        pitch=math.degrees(msg.pitch),
        yaw=math.degrees(msg.yaw),
    )
```

### 2. 引入参数对象（Introduce Parameter Object）

**触发**: 函数参数 > 3 个 / 多个函数共享相同参数

**Before**:
```python
def goto(self, lat: float, lon: float, alt: float, speed: float) -> Result:
    ...
```

**After**:
```python
@dataclass
class Target:
    lat: float
    lon: float
    alt: float
    speed: float = 5.0

def goto(self, target: Target) -> Result:
    ...
```

### 3. 用 dataclass 替代 dict

**Before**:
```python
data = {
    "name": "foo",
    "value": 0.0,
    "enabled": False,
}
```

**After**:
```python
@dataclass
class Config:
    name: str
    value: float
    enabled: bool = False

data = Config(name="foo", value=0.0)
```

## 禁止事项

- [ ] 禁止删除已有 docstring（即使冗余）
- [ ] 禁止删除已有注释（即使过时，先标 TODO）
- [ ] 禁止改变函数签名（除非调用方同步更新）
- [ ] 禁止改变公开 API 行为
- [ ] 禁止跳过测试验证
- [ ] 禁止在重构中"顺便"修 bug（分开做）
- [ ] 禁止引入新依赖（除非必要且有充分理由）

## 回滚策略

如有疑问，立即回滚：
```bash
git checkout -- path/to/changed/file.py
# 或
git reset --hard HEAD
```

## 输出格式

```markdown
## Refactor 建议

**文件**: `<file_path>`
**目标**: <重构目标>

### 识别的 Smell

1. **大函数** `process()` 50 行
2. **重复代码** `_parse_xxx` 在 3 处复制
3. **类型不安全** `data` 用 dict 传递

### 建议的重构

1. **提取函数** `process` → `_parse_a` + `_parse_b` + 主函数
2. **引入 dataclass** `data` dict → `Data` dataclass
3. **DRY** 提取公共方法

### 重构顺序（风险递增）

1. 先做 1（最小风险）
2. 再做 2（中风险，需更新所有调用方）
3. 最后做 3

### 每步验证

- 跑 `pytest tests/`
- 跑 `git diff --stat`

### 风险评估

- **风险等级**: 🟢 低 / 🟡 中 / 🔴 高
- **影响范围**: <file_count> 个文件
- **行为变化**: 无（行为不变）
```
