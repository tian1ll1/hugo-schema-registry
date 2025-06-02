# Spring Batch Grafana Dashboard

本项目提供了一个基于 Prometheus 的 Spring Batch 监控 Grafana 仪表盘配置文件（spring-batch-grafana-dashboard.json），用于实时监控和分析 Spring Batch 作业与 Step 的运行状态和性能。

## 仪表盘主要功能

- **作业成功与失败统计**：展示最近 24 小时内所有作业的成功与失败数量（饼图）。
- **当前正在运行的作业**：实时显示所有正在运行的作业（表格）。
- **作业平均耗时**：按作业名分组，展示各作业的平均执行时间（柱状图）。
- **作业执行次数趋势**：统计每小时各作业的启动次数（折线图）。
- **Step 平均耗时**：按 Step 名分组，展示各 Step 的平均执行时间（柱状图）。
- **Step 成功与失败统计**：统计最近 24 小时各 Step 的成功与失败数量（饼图）。
- **最近失败的作业/Step 列表**：列出当前失败的作业和 Step（表格）。

## 使用方法

1. **前提条件**
   - 已部署 Prometheus 并采集了 Spring Batch 相关指标（推荐使用 Micrometer）。
   - 已安装并运行 Grafana（建议 8.x 及以上版本）。
   - Prometheus 已作为数据源添加到 Grafana。

2. **导入仪表盘**
   - 打开 Grafana，进入"Dashboards" > "Import"。
   - 上传 `spring-batch-grafana-dashboard.json` 文件，或将文件内容粘贴到导入窗口。
   - 选择 Prometheus 数据源，点击"Import"完成导入。

3. **查看与自定义**
   - 仪表盘导入后即可实时查看各项监控数据。
   - 可根据实际需求调整面板布局、查询表达式或添加更多监控项。

## Prometheus 指标说明

- `spring_batch_job_duration_seconds_sum` / `spring_batch_job_duration_seconds_count`：作业耗时统计
- `spring_batch_job_active`：当前活跃作业数
- `spring_batch_job{status="FAILED"}`：失败作业
- `spring_batch_step_duration_seconds_sum` / `spring_batch_step_duration_seconds_count`：Step 耗时统计
- `spring_batch_step{status="FAILED"}`：失败 Step
- 其他指标请参考 Micrometer 官方文档

## 典型面板查询示例

- 作业平均耗时：
  ```
  sum by (job) (rate(spring_batch_job_duration_seconds_sum[5m])) / sum by (job) (rate(spring_batch_job_duration_seconds_count[5m]))
  ```
- Step 平均耗时：
  ```
  sum by (job, step) (rate(spring_batch_step_duration_seconds_sum[5m])) / sum by (job, step) (rate(spring_batch_step_duration_seconds_count[5m]))
  ```
- Step 成功与失败统计：
  ```
  sum by (job, step) (increase(spring_batch_step{status="COMPLETED"}[1d]))
  sum by (job, step) (increase(spring_batch_step{status="FAILED"}[1d]))
  ```

## 参考

- [Spring Batch 官方文档](https://docs.spring.io/spring-batch/)
- [Micrometer 官方文档](https://micrometer.io/docs)
- [Grafana 官方文档](https://grafana.com/docs/)
- [Prometheus 官方文档](https://prometheus.io/docs/)

---

如需进一步自定义仪表盘或有其他监控需求，欢迎随时提问！

---

# English Version

## Project Overview

This project provides a Prometheus-based Spring Batch monitoring Grafana dashboard configuration file (`spring-batch-grafana-dashboard.json`) for real-time monitoring and analysis of Spring Batch job and step execution status and performance.

## Main Features

- **Job Success vs Failure**: Pie chart showing the number of successful and failed jobs in the last 24 hours.
- **Currently Running Jobs**: Table listing all currently running jobs in real time.
- **Job Average Duration**: Bar chart showing the average execution time of each job (grouped by job name).
- **Job Execution Count Trend**: Line chart showing the number of job executions per hour.
- **Step Average Duration**: Bar chart showing the average execution time of each step (grouped by step name).
- **Step Success vs Failure**: Pie chart showing the number of successful and failed steps in the last 24 hours.
- **Recent Failed Jobs/Steps**: Table listing all currently failed jobs and steps.

## Usage

1. **Prerequisites**
   - Prometheus is deployed and collecting Spring Batch metrics (Micrometer is recommended).
   - Grafana is installed and running (version 8.x or above recommended).
   - Prometheus is added as a data source in Grafana.

2. **Import the Dashboard**
   - Open Grafana and go to "Dashboards" > "Import".
   - Upload the `spring-batch-grafana-dashboard.json` file or paste its content into the import window.
   - Select the Prometheus data source and click "Import" to finish.

3. **View and Customize**
   - After importing, you can view all monitoring data in real time.
   - You can adjust panel layout, query expressions, or add more panels as needed.

## Prometheus Metrics

- `spring_batch_job_duration_seconds_sum` / `spring_batch_job_duration_seconds_count`: Job duration statistics
- `spring_batch_job_active`: Number of currently active jobs
- `spring_batch_job{status="FAILED"}`: Failed jobs
- `spring_batch_step_duration_seconds_sum` / `spring_batch_step_duration_seconds_count`: Step duration statistics
- `spring_batch_step{status="FAILED"}`: Failed steps
- For more metrics, refer to the Micrometer documentation

## Example Panel Queries

- Job Average Duration:
  ```
  sum by (job) (rate(spring_batch_job_duration_seconds_sum[5m])) / sum by (job) (rate(spring_batch_job_duration_seconds_count[5m]))
  ```
- Step Average Duration:
  ```
  sum by (job, step) (rate(spring_batch_step_duration_seconds_sum[5m])) / sum by (job, step) (rate(spring_batch_step_duration_seconds_count[5m]))
  ```
- Step Success vs Failure:
  ```
  sum by (job, step) (increase(spring_batch_step{status="COMPLETED"}[1d]))
  sum by (job, step) (increase(spring_batch_step{status="FAILED"}[1d]))
  ```

## References

- [Spring Batch Documentation](https://docs.spring.io/spring-batch/)
- [Micrometer Documentation](https://micrometer.io/docs)
- [Grafana Documentation](https://grafana.com/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/)

---

Feel free to ask if you need further customization or have other monitoring requirements! 