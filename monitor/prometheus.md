# prometheus 

## 指标类型
+ counter（计数型）：只增不减
+ gauge（测量型）：上下增长的数字。虽然永远不会减少，但有时可以将其重置为0并开始递增。
+ histogram（直方图）：不同区间内样本个数
+ summary（摘要）：与 Histogram 类型类似，用于表示一段时间内的数据采样结果（通常是请求持续时间或响应大小等），但它直接存储了分位数（通过客户端计算，然后展示出来），而不是通过区间来计算。

## 监控方法论USE
针对每个资源，检查使用率、饱和度和错误。

# promql

## 计算过去5分钟 gauge类型指标过去5分钟内的总和
```yaml
sum(
  sum_over_time(foo{bar="xxx"}[5m])
)
```

在grafna中，不想把时间固定死，可以使用$__range这个变量

```yaml
sum(
  sum_over_time(foo{bar="xxx"}[$__range])
)
```

## 计算过去5分钟 gauge类型指标过去5分钟内的最大值

```yaml
max_over_time(foo[5m])
```