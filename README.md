# Delivery Time
## Notice
It is possible that GitHub fails to display Jupyter Notebooks. Should such circumstances arise, please refer to ***Part 4. Steps*** listed below for code samples.

## Part 1. Objective
To calculate delivery time with out-of-hours exclusion.

## Part 2. Data
### 2.1. Background Information
- Business hours: **D1** 8:00 - **D1** 21:00
- Out of hours: **D1** 21:00 - **D2** 8:00

### 2.2. Original Data
| Order          | Delivery       |
| :---           | :---           |
| 2020/1/1 14:00 | 2020/1/8 11:00 |
| 2020/1/1 14:00 | 2020/1/8 7:00  |
| 2020/1/1 14:00 | 2020/1/2 11:00 |
| 2020/1/1 23:00 | 2020/1/8 11:00 |
| 2020/1/1 23:00 | 2020/1/8 7:00  |
| 2020/1/1 23:00 | 2020/1/2 11:00 |
| 2020/1/1 9:00  | 2020/1/1 18:00 |

### 2.3. Expected Result
| Order          | Delivery       | Delivery Time  |
| :---           | :---           | :---           |
| 2020/1/1 14:00 | 2020/1/8 11:00 | 88h            |
| 2020/1/1 14:00 | 2020/1/8 7:00  | 85h            |
| 2020/1/1 14:00 | 2020/1/2 11:00 | 10h            |
| 2020/1/1 23:00 | 2020/1/8 11:00 | 81h            |
| 2020/1/1 23:00 | 2020/1/8 7:00  | 78h            |
| 2020/1/1 23:00 | 2020/1/2 11:00 | 3h             |
| 2020/1/1 9:00  | 2020/1/1 18:00 | 9h             |

## Part 3. Outline
- According to when the order is made and when the delivery is completed, there are ***3 circumstances*** to be taken into account when calculating delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_0_Circumstances.png"/></div>

- There are 2 ways to calculate the expected delivery time:
### 3.1. Time Addition
- Add up eligible delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_1_TimeAddition.png"/></div>

```
def time_addition(order, delivery):
    order_date, order_time = order.date(), order.time()
    delivery_date, delivery_time = delivery.date(), delivery.time()
    
    order_21 = datetime.datetime(order.year, order.month, order.day, 21, 0)
    delivery_8 = datetime.datetime(delivery.year, delivery.month, delivery.day, 8, 0) 
    
    if order_date != delivery_date:
        first_day_duration = max((order_21-order).total_seconds() / 3600, 0)
        last_day_duration = max((delivery-delivery_8).total_seconds() / 3600, 0)
        delivery_duration = (delivery_date-order_date).days-1
        total_duration = first_day_duration + last_day_duration + delivery_duration*13        
    elif order_date == delivery_date:
        total_duration = (delivery-order).total_seconds() / 3600
        
    return total_duration
```

### 3.2. Time Deduction
- Deduct ineligible delivery time.

<div align=center><img src="https://github.com/lclh813/Delivery_Time/blob/master/Pic/P_2_TimeDeduction.png"/></div>

```
def time_deduction(order, delivery):
    order_date, order_time = order.date(), order.time()
    delivery_date, delivery_time = delivery.date(), delivery.time()
    
    order_21 = datetime.datetime(order.year, order.month, order.day, 21, 0)
    delivery_8 = datetime.datetime(delivery.year, delivery.month, delivery.day, 8, 0)
    
    duration = (delivery-order).total_seconds() / 3600    
    deduction = (delivery_date-order_date).days 
    
    first_day_deduction = min((order_21-order).total_seconds() / 3600, 0)
    last_day_deduction = min((delivery-delivery_8).total_seconds() / 3600, 0)
    
    total_duration = duration - first_day_deduction - last_day_deduction - deduction*11
    
    return total_duration
```
