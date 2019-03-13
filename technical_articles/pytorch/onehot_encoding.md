# Onehot encoding

## scatter

```python
indices = torch.LongTensor((1, 3, 5, 9))
indices = indices.view(-1, 1)
onehot = torch.zeros(4, 10)
onehot.scatter(-1, indices, 1)
```



