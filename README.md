## 0309 
### 加入 SustainDC: Benchmarking for Sustainable Data Center Control 元素

#### 電池管理 (BatteryManager)
- 新增 `BatteryManager` 類，模擬電池的充電和放電行為。
- 根據 `hourly_carbon_list` 決定充電策略：當碳排放指數低於 0.55 時充電，否則放電。
- 將電池狀態 (`battery_level_over_time`) 存儲在 `results` 中，並在視覺化中展示。
  
#### 根據太陽能功率調整活躍節點數量
- 使用 `solar_watt_list` 動態調整活躍節點數量：
  - `active_node_limit = max(MIN_IDLE_HOSTS, int(len(nodes) * (solar_power / max(solar_watt_list))))`。
- 在每個調度步驟中，限制可用節點數量為 `active_nodes`，以反映太陽能功率的影響。

#### 計算等待時間
- 在 `eavmat_scheduling` 中，記錄每個工作的提交時間 (`start_time`) 和調度時間 (`scheduled_time`)，計算等待時間並存儲在 `results["waiting_times"]` 中。
- 在 `pso_scheduling` 中，由於 PSO 是全局優化，假設調度後立即執行，等待時間為 0。
- 計算平均等待時間並存儲在 `results["average_waiting_time"]` 中。

#### 數據整合
- 使用 `CPU_request_time` 對齊時間點，從 `solar_watt_list` 和 `hourly_carbon_list` 中提取對應值。
- 由於 `workload_list` 和 `PDU_DATA` 時間範圍較長，當前僅用於參考，未直接整合到調度中。

### 預期結果

#### 等待時間統計
- 每個算法的 `results` 包含 `waiting_times`（每個工作的等待時間列表）和 `average_waiting_time`（平均等待時間）。
- 在輸出中顯示每個算法的平均等待時間，例如：

```text
EAVMAT Scheduling Results:
Average Waiting Time (minutes): 0.00

PSO Scheduling Results:
Average Waiting Time (minutes): 0.00

