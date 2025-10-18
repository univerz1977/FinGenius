# DeepSeek API 集成指南

本文档介绍如何在 FinGenius 项目中配置和使用 DeepSeek API。

## 配置方法

### 1. 基础配置

在 `config/config.toml` 文件中添加以下配置：

```toml
[llm]
api_type = "openai"  # DeepSeek 使用 OpenAI 兼容格式
model = "deepseek-chat"  # 可选模型：deepseek-chat, deepseek-coder, deepseek-reasoner
base_url = "https://api.deepseek.com/v1"
api_key = "YOUR_DEEPSEEK_API_KEY"  # 替换为你的 API 密钥
max_tokens = 8192
temperature = 0.0
```

### 2. 特定模型配置

如果需要为特定任务配置不同的 DeepSeek 模型：

```toml
# 默认配置
[llm]
api_type = "openai"
model = "deepseek-chat"
base_url = "https://api.deepseek.com/v1"
api_key = "YOUR_DEEPSEEK_API_KEY"
max_tokens = 8192
temperature = 0.0

# 推理模型配置
[llm.reasoning]
api_type = "openai"
model = "deepseek-reasoner"  # 推理专用模型
base_url = "https://api.deepseek.com/v1"
api_key = "YOUR_DEEPSEEK_API_KEY"
max_tokens = 8192
temperature = 0.0

# 代码分析配置
[llm.coding]
api_type = "openai"
model = "deepseek-coder"  # 代码专用模型
base_url = "https://api.deepseek.com/v1"
api_key = "YOUR_DEEPSEEK_API_KEY"
max_tokens = 8192
temperature = 0.0
```

## 支持的 DeepSeek 模型

### 1. 对话模型
- `deepseek-chat`：通用对话模型，适合大多数金融分析场景
- 支持工具调用、多轮对话
- 对中文理解优秀，适合 A 股分析

### 2. 代码模型
- `deepseek-coder`：代码生成和理解专用
- 适合技术分析相关的代码生成

### 3. 推理模型
- `deepseek-reasoner`：推理专用模型
- 适合复杂的逻辑推理和分析
- 在博弈环境中表现优秀

## 功能特性

### 1. 完全兼容 OpenAI API
- 使用标准的 OpenAI SDK
- 支持流式和非流式响应
- 支持工具调用（Function Calling）
- 支持多模态输入

### 2. 中文优化
- 对中文金融术语理解更佳
- 在 A 股分析场景中表现优秀
- 支持中文工具调用

### 3. 成本优势
- 相比其他商业 API 更具性价比
- 适合大规模金融分析任务

## 使用示例

### 基础股票分析
```bash
python main.py 000001 --llm-config deepseek
```

### 使用推理模型
```bash
python main.py 000001 --llm-config reasoning
```

## 注意事项

1. **API 密钥**：需要从 DeepSeek 官网申请 API 密钥
2. **速率限制**：注意 API 调用频率限制
3. **模型选择**：根据任务类型选择合适的模型
4. **Token 限制**：DeepSeek 支持最大 128K 上下文

## 故障排除

### 常见问题

1. **认证失败**
   - 检查 API 密钥是否正确
   - 确认 base_url 为 `https://api.deepseek.com/v1`

2. **模型不可用**
   - 确认模型名称拼写正确
   - 检查模型是否在服务中

3. **工具调用失败**
   - DeepSeek 完全支持 OpenAI 格式的工具调用
   - 检查工具定义是否符合规范

## 性能优化建议

1. **批量处理**：对于大量股票分析，考虑批量请求
2. **缓存结果**：重复分析可以缓存结果减少 API 调用
3. **合理配置**：根据任务复杂度选择合适的模型和参数