## 1. RandomForest
- 选7天作为训练数据，预测未来1天
- 再选7天作为训练数据，预测未来1天
- ...

```python
data = df.copy()
n = 12
for i in range(1, n+1):
    data['ypre_'+str(i)] = data['y'].shift(i)
data = data[['ds']+['ypre_'+str(i) for i in range(n, 0, -1)]+['y']]

# 提取训练集和测试集
X_train = data[data['ds']<"19580101"].dropna()[['ypre_'+str(i) for i in range(n, 0, -1)]]
y_train = data[data['ds']<"19580101"].dropna()[['y']]
X_test = data[data['ds']>="19580101"].dropna()[['ypre_'+str(i) for i in range(n, 0, -1)]]
y_test = data[data['ds']>="19580101"].dropna()[['y']]

# 模型训练和预测
rf = RandomForestRegressor(n_estimators=10, max_depth=5)
rf.fit(X_train, y_train)
y_pred = rf.predict(X_test)

# 结果对比绘图
y_test.assign(yhat=y_pred).plot()
```