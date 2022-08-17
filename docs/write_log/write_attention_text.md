可视化文本Transformer的注意力值。选择需要可视化的文本数据，可视化并统计其对应的注意力值

```python
summarywriter.add_attention(tag: str,
                               texts: list,
                               tokenizer_tokens: list,
                               attention_data: list,
                               model_type: str):
    """
    	添加文本Transformer需要可视化的句子及其对应的的注意力数据到日志
    Args:
    	tag:字符串，Transformer可视化的标识。
    	texts: 列表，需要可视化的句子。
    	tokenizer_tokens:列表，需要可视化的句子经过分词后的token列表。
    	attention_data:列表，需要可视化的句子在模型中所对应的注意力数据。
    	model_type:字符串，若为"gpt2"，则模型为非双向的。其他的模型为双向的。
    """
```

