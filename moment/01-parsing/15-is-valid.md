---
title: isValid()
version: 1.7.0
signature: |
  moment().isValid();
---


Moment 比 `Date` 构造器应用更严格的初始化规则。

```js
new Date(2013, 25, 14).toString(); // "Sat Feb 14 2015 00:00:00 GMT-0500 (EST)"
moment([2015, 25, 35]).format();   // 'Invalid date'
```

可以使用 `moment#isValid` 检查 Moment 是否被视为无效的日期。
可以使用 `moment#parsingFlags` 查看 `#isValid` 使用的指标，该指标返回一个对象。

无效的日期会返回以下的解析标志：

 * `overflow`: 日期字段的溢出，例如第 13 个月、月份的第 32 天（或非闰年的 2 月 29 日）、年份的第 367 天等。
   `overflow` 会包含匹配 `#invalidAt`（参阅下文）的无效单位的索引，`-1` 表示没有溢出。
 * `invalidMonth`: 无效的月份名称，例如 ```moment('Marbruary', 'MMMM');```。
   会包含无效的月份字符串本身或 null。
 * `empty`: 包含无法解析任何内容的输入字符串，例如 `moment('this is nonsense');`。布尔值。
 * `nullInput`: 输入 `null`，例如 `moment(null);`。布尔值。
 * `invalidFormat`: 空的格式列表，例如 `moment('2013-05-25', [])`。布尔值。
 * `userInvalidated`: 显式地创建为无效的日期，例如 `moment.invalid()`。布尔值。

 除上述内容外，从 **2.13.0** 开始，meridiem 和 parsedDateParts 标志也可以一起判断日期的有效性。
 
 * `meridiem`: 表明解析的子午线（AM/PM），如果有的话。字符串。
 * `parsedDateParts`: 返回按降序解析的日期片段的数组，即 parsedDateParts[0] === 年份。 如果没有片段，但子午线有值，则日期无效。数组。

此外，如果在严格模式中解析 Moment，则这些标志必须为空才能使 Moment 有效：

 * `unusedTokens`: 在输入的字符串中找不到格式数组的子字符串。
 * `unusedInput`: 输入的子字符串数组与格式字符串不匹配。

注意：Moment 的有效性概念在 **2.2** 和 **2.3** 之间变得更加严格和一致。 
注意：有效性在创建 moment 时确定。 
修改的 moment（即 `moment().hour(NaN)`）将会保持有效。

此外，可以使用 `moment#invalidAt` 来确定溢出的是哪个日期单位。

```javascript
var m = moment("2011-10-10T10:20:90");
m.isValid(); // false
m.invalidAt(); // 5 表示秒钟
```

返回值具有以下含义：

<ol>
  <li>年份</li>
  <li>月份</li>
  <li>日期</li>
  <li>小时</li>
  <li>分钟</li>
  <li>秒钟</li>
  <li>毫秒</li>
</ol>

注意：如果有多个错误的单位，则返回第一个单位（例如，由于日期的有效性可能取决于月份）。

无效的 Moment
===============

如果 moment 无效，则在浮点运算中的行为类似于 NaN。

以下所有的都会生成无效的 moment：

* `invalid.add(unit, value)`
* `another.add(invalid)`
* `invalid.clone()`
* `invalid.diff(another)`
* `invalid.endOf(unit)`
* `invalid.max(another)`
* `another.max(invalid)`
* `invalid.min(another)`
* `another.min(invalid)`
* `invalid.set(unit, value)`
* `invalid.startOf(unit)`
* `invalid.subtract(unit, value)`

以下会生成 `'InvalidDate'` 的本地化版本：

* `invalid.format(anyFmt)` 在当前语言环境中导致 `'Invalid Date'`。
* `invalid.from(another)`
* `another.from(invalid)`
* `invalid.fromNow(suffix)`
* `invalid.to(another)`
* `another.to(invalid)`
* `invalid.toNow(suffix)`
* `invalid.toISOString()` (在 2.18.0 之前)
* `invalid.toString()`

以下会返回 `false`：

* `invalid.isAfter(another)`
* `invalid.isAfter(invalid)`
* `another.isAfter(invalid)`
* `invalid.isBefore(another)`
* `invalid.isBefore(invalid)`
* `another.isBefore(invalid)`
* `invalid.isBetween(another, another)`
* `invalid.isBetween(invalid, invalid)`
* `invalid.isSame(another)`
* `invalid.isSame(invalid)`
* `another.isSame(invalid)`
* `invalid.isSameOrAfter(another)`
* `invalid.isSameOrAfter(invalid)`
* `another.isSameOrAfter(invalid)`
* `invalid.isSameOrBefore(another)`
* `invalid.isSameOrBefore(invalid)`
* `another.isSameOrBefore(invalid)`

这些会返回具有某些结构的 `null` 或 `NaN`：

* `invalid.get(unit)` 返回 null，就像其他所有命名的 getter 一样。
* `invalid.toArray() === [NaN, NaN, NaN, NaN, NaN, NaN]`
* `invalid.toObject()` 的所有值都被设置为 `NaN`。
* `invalid.toDate()` 返回无效的 Date 对象。
* `invalid.toJSON()` 返回 null。
* `invalid.unix()` 返回 null。
* `invalid.valueOf()` 返回 null。
* `invalid.toISOString()` 返回 null（从 2.18.0 开始）。
