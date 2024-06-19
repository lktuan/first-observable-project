# Data Loader: a local weather forecast

**I just display the data**

```js
const forecast = FileAttachment("./data/forecast.json").json();
```

```js
display(Inputs.table(forecast.properties.periods));
```