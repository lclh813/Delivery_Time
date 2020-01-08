# Delivery Time
## Notice
It is possible that GitHub fails to display Jupyter Notebooks. Should such circumstances arise, please refer to ***Part 4. Steps*** listed below for code samples.

## Part 1. Objective
To calculate delivery time with out-of-hours exclusion.

## Part 2. Data
### 2.1. Background Information
- Business hours: D1 8:00 - D1 21:00
- Out of hours: D1 21:00 - D2 8:00

### 2.2. Original Data
| Order Log      | Delivery Log   |
| :---           | :---           |
| 2020/1/1 14:00 | 2020/1/8 11:00 |
| 2020/1/1 14:00 | 2020/1/8 7:00  |
| 2020/1/1 14:00 | 2020/1/2 11:00 |
| 2020/1/1 23:00 | 2020/1/8 11:00 |
| 2020/1/1 23:00 | 2020/1/8 7:00  |
| 2020/1/1 23:00 | 2020/1/2 11:00 |
| 2020/1/1 9:00  | 2020/1/1 18:00 |

### 2.3. Expected Result
| Order Log      | Delivery Log   | Delivery Time  |
| :---           | :---           | :---           |
| 2020/1/1 14:00 | 2020/1/8 11:00 | 88h            |
| 2020/1/1 14:00 | 2020/1/8 7:00  | 85h            |
| 2020/1/1 14:00 | 2020/1/2 11:00 | 10h            |
| 2020/1/1 23:00 | 2020/1/8 11:00 | 81h            |
| 2020/1/1 23:00 | 2020/1/8 7:00  | 78h            |
| 2020/1/1 23:00 | 2020/1/2 11:00 | 3h             |
| 2020/1/1 9:00  | 2020/1/1 18:00 | 9h             |

- According to when the ***order*** is made and when the ***delivery*** is completed, there are 3 scenarioss to be taken into account when calculating delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_0_Circumstances.png"/></div>

- There are 2 ways to calculate the expected delivery time:
### 2.3.1. Time Addition
- Add up eligible delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_1_TimeAddition.png"/></div>

### 2.3.2. Time Deduction
- Deduct ineligible delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_2_TimeDeduction.png"/></div>

