**scikit-learn syntax**
```python
from sklearn.module import Model
model = Model()
model.fit(X, y)
predictions = model.predict(X_new)
print(predictions)
```

- scikit-learn requires that *each column is a feature, and each row a different observation*, `X = df[[feature1, feature2]].values` accomplishes this, the `.values` attribute turns the values into a numpy array. `shape = (n_obs, n_features)`

$$Accuracy = \frac{correct\ predictions}{total\ observations}$$
