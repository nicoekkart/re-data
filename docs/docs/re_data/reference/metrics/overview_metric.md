---
sidebar_position: 0
dbt_docs_base_url: https://re-data.github.io/dbt-re-data
---


# Overview

## How metrics look like 

re_data metrics are currently *just* expressions which
are added to select statements run automatically by re_data.

```sql title="re_data query"
select metric1, metric2, metric3
from your_table
where data in time_window
```

These simple definitions still make it possible to create a wide variety of metrics.
In case metric is more than single sql expression, you can also create them by using sub queries in metric macros (more details in custom metrics section)

## Time based

We recommend that most of your metrics computed would be time-based (data is then filtered by the `time_filter` specified in the table config.
`time_filter` can be either some date column comparable to timestamp or SQL expression that will be comparable to the timestamp in your data warehouse. *(And if you think we can shorten this definition to just SQL expression as column name is one, you are right 😊*

## Global

In cases when time-based filtering is not possible re_data can compute global metrics for a table. Global metrics don't filter by time and work on data from the whole table. You can pass `time_filter: null` in the re_data table config to compute global metrics.

## Table level

Table level metrics compute stats based on the whole table row, the most simple example of this is `row_count` metric. Your custom table level metrics can use multiple columns when computing the value.

## Column level

Column level metrics are testing a single column of data values. For example, computing maximal value appears in the column. They take column names as an argument, which makes them generic. (you can use them on different columns and different tables)

## Default 

re_data comes with a set of metrics that are computed by default for all monitored tables. This is controlled by `re_data:default_metrics`. Default metrics variable contain list of metrics groups which you would like to compute for all the tables. Check out 

```sql title="re_data:default_metrics:"
  re_data:metrics_groups:
    table_metrics:
      table:
        - row_count
        - freshness

    column_metrics:
      column:
        numeric:
          - min
          - max
          - avg
          - stddev
          - variance
          - nulls_count
          - nulls_percent
        text:
          - min_length
          - max_length
          - avg_length
          - nulls_count
          - missing_count
          - nulls_percent
          - missing_percent

  re_data:default_metrics:
    - table_metrics
    - column_metrics

```


Definition of all base metrics is available under **[default metrics](/docs/re_data/reference/metrics/base_metrics)** section.

## Extra

Apart from base metrics which can be added to your metrics computed, but are not available computed by default. Full list of those metrics is available in **[Extra metrics](/docs/re_data/reference/metrics/extra_metrics)** section.

## Custom

re_data makes it possible to create macros which will compute your own metrics. More information about that in **[Custom metrics](/docs/re_data/reference/metrics/your_own_metric)**  section.

