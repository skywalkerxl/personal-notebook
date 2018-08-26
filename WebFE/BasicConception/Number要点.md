# Number要点

### 1. NaN 

1. NaN与任何值都不相等，包括NaN本身

   ```
   NaN == NaN  // false
   NaN === NaN  // false
   ```

2. isNaNk可以帮助确定是否“不是数值”

   ```
   isNaN(NaN)    // true
   isNaN(10)     // false
   isNaN("10")   // false(因为这里可以被转换成数值)
   isNaN("blue")     // true(不能被转换成数值)
   isNaN(true)       // false(可以被转换成数值1)
   isNaN("")     // false()
   ```

3. 一般 0 除以 0 才会返回NaN，正整数除以0返回Infinity, 负整数除以0返回-Infinity



### 2. 数值转换

1. Number()
2. parseInt()
3. parseFloat()