---
sql:
  gaia: ./lib/gaia-sample.parquet
toc: false
---

# 2. SQL in Observable: Gaia

To run SQL queries, create a SQL fenced code block (<code>```sql</code>). For example, to query the first 10 rows from the `gaia` table:

````md
```sql
SELECT * FROM gaia ORDER BY phot_g_mean_mag LIMIT 10
```
````

This produces a table:

```sql
SELECT * FROM gaia ORDER BY phot_g_mean_mag LIMIT 10
```

We can assign an `id` to the code block header, we can refer to that table later. But the output will be no longer displayed, to display it, we add `display` option after `id`.

````md
```sql id=top10
SELECT * FROM gaia ORDER BY phot_g_mean_mag LIMIT 10
```
````

```sql id=top10
SELECT * FROM gaia ORDER BY phot_g_mean_mag LIMIT 10
```

Refer `top10` in a `js` block code:
````md
```js
top10
```
````

```js
top10
```

# Plot

We can bin the stars by right [ascension](https://en.wikipedia.org/wiki/Right_ascension) (ra) and [declination](https://en.wikipedia.org/wiki/Declination) (dec), then plot like this:

```md
SELECT
  floor(ra / 2) * 2 + 1 AS ra,
  floor(dec / 2) * 2 + 1 AS dec,
  count() AS count
FROM
  gaia
GROUP BY
  1,
  2
```

```sql id=bins display
SELECT
  floor(ra / 2) * 2 + 1 AS ra,
  floor(dec / 2) * 2 + 1 AS dec,
  count() AS count
FROM
  gaia
GROUP BY
  1,
  2
```

```js
Plot.plot({
  aspectRatio: 1,
  x: {domain: [0, 360]},
  y: {domain: [-90, 90]},
  marks: [
    Plot.frame({fill: 0}),
    Plot.raster(bins, {
      x: "ra",
      y: "dec",
      fill: "count",
      width: 360 / 2,
      height: 180 / 2,
      imageRendering: "pixelated"
    })
  ]
})
```