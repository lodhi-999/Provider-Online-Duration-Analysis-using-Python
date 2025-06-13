#  Provider Online Duration Analysis using Python

This project analyzes provider online durations from a raw dataset containing **1.9 million rows** and 4 columns. It focuses on event classification, time-based filtering, and calculation of total active (online) time per provider on an hourly basis.

---

## Dataset Overview

- **Size**: ~1.9 million rows
- **Columns**:
  - `provider_id`
  - `detail` (contains dictionary-like strings)
  - `source`
  - `event_time`

---

##  Steps Performed

### 1. **Data Loading & Parsing**
- Loaded raw data and parsed the `event_time` column into datetime format.
- Extracted values from `detail`, which contained dictionary-like strings.

### 2. **Filtering Valid Events**
- Only retained:
  - `Event 4` (Online) — if `detail` contains `"True"` or `source` contains `"Action on Job"`.
  - `Event 5` (Offline) — if `detail` contains `"False"`.
- All other event types were treated as errors and excluded.

### 3. **Time-Based Filtering**
- Excluded all events that occurred **after 7:00 PM** (i.e., `hour >= 19`).

### 4. **Status Classification**
- Each event was classified as `online`, `offline`, or `ignore` based on the `detail` and `source` content.

### 5. **Online Time Calculation**
- Data sorted by `provider_id` and `event_time`.
- Used vectorized `.shift()` to access next event's time/status.
- Calculated session duration only for valid online events (capped at 3600 seconds per hour).
- Aggregated **total online seconds** per provider, per hourly slot.

### 6. **Final Output**
- Structured output includes:
  - `provider_id`
  - `date` (formatted as `DD-MMM`)
  - `hour_start`, `hour_end`
  - `seconds_online`

---

##  Final Stats

| Metric                                 | Value        |
|----------------------------------------|--------------|
| Online Events                          | 176,836      |
| Offline Events                         | 518,831      |
| Unique Providers (after cleaning)      | 14,914       |
| Providers after 7 PM (removed)         | 14,914       |
| Expected Hourly Output Rows            | 4,921,620    |
| Non-zero Online Time Entries           | 73,531       |

---

## 🛠 Tools Used

- Python (Pandas, NumPy)
- Datetime operations
- Vectorized data manipulation

---

##  Outcome

The final dataset provides a reliable, per-hour breakdown of each provider’s online presence — useful for behavioral tracking, operational reporting, or performance analysis.
