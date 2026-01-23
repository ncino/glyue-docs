# Data Manipulation Utilities

## List of Functions <a href="#list-of-functions" id="list-of-functions"></a>

* [extract\_chars()](data-manipulation-utilities.md#extract_chars)
* [extract\_digits()](data-manipulation-utilities.md#extract_digits)
* [percent\_to\_decimal()](data-manipulation-utilities.md#percent_to_decimal)
* [round\_half\_up()](data-manipulation-utilities.md#round_half_up)
* [get\_current\_date\_string()](data-manipulation-utilities.md#get_current_date_string)
* [convert\_date\_string()](data-manipulation-utilities.md#convert_date_string)
* [IndexGenerator](data-manipulation-utilities.md#indexgenerator)

## Getting Started <a href="#getting-started" id="getting-started"></a>

```python
import_helper("data_utils")
```

Import the `data_utils` integration helper in the Integration _Before Hook_ to expose the helper functions to the integration’s namespace.

### extract\_chars() <a href="#extract_chars" id="extract_chars"></a>

```python
data_utils.extract_chars(field, start=0, stop=None, step=None)
```

Takes `str` or `Flex(None)` input and returns spliced output using typical python string splicing logic. Returns `None` for `Flex(None)`

```python
data_utils.extract_chars("test123abc", 4) -> "test" 
data_utils.extract_chars("test123abc", 7, 100) -> "abc" 
data_utils.extract_chars("test123abc", 0, 7, 2) -> "ts13" 
data_utils.extract_chars("") -> "" 
data_utils.extract_chars(None, 0, 7, 2) -> None
```

### extract\_digits() <a href="#extract_digits" id="extract_digits"></a>

```python
data_utils.extract_digits(field)
```

Given a `str` or `Flex(None)` input, and outputs all digits.

```python
data_utils.extract_digits("abc123") -> "123"
data_utils.extract_digits("123-456-7890") -> "1234567890"
data_utils.extract_digits("") -> ""
data_utils.extract_digits(None) -> None
```

### percent\_to\_decimal() <a href="#percent_to_decimal" id="percent_to_decimal"></a>

```python
data_utils.percent_to_decimal(percent)
```

Given an `int`, `float`, `Decimal`, or number in `str` format and returns value in decimal format.

```python
data_utils.percent_to_decimal(8) -> Decimal("0.08")
data_utils.percent_to_decimal("") -> None
data_utils.percent_to_decimal(None) -> None
```

### round\_half\_up() <a href="#round_half_up" id="round_half_up"></a>

```python
data_utils.round_half_up(input_num, decimal_places=2)
```

Given an input of `float`, `int`, `Decimal`, or number in `str` format, outputs a `Decimal` number rounded to n decimal places. This implements the typical round half up strategy where 0.5 rounds up.

```python
data_utils.round_half_up(0.875, decimal_places=2) -> Decimal("0.88")
data_utils.round_half_up("", decimal_places=2) -> None
data_utils.round_half_up(None, decimal_places=2) -> None
```

### get\_current\_date\_string() <a href="#get_current_date_string" id="get_current_date_string"></a>

```python
data_utils.get_current_date_string(timezone, date_format="%Y-%m-%d")
```

Given a time zone and datetime format input, returns the current date `str` in the specified format.

```python
data_utils.get_current_date_string("US/Pacific", "%Y-%m-%d") -> "2024-12-22"
```

All timezones in the pytz package are supported. Here are the major US time zones:

* `US/Central`
* `US/Eastern`
* `US/Mountain`
* `US/Pacific`

You can use the following Python to capture the full list:

```python
import pytz

# total number of timezones
len(pytz.all_timezones)

# list all timezones
for tz in pytz.all_timezones:
    print(tz)
```

### convert\_date\_string() <a href="#convert_date_string" id="convert_date_string"></a>

```python
data_utils.convert_date_string(input_date, in_format, out_format="%Y-%m-%d")
```

Given a date string and it’s date time format, converts to the specified output format as a date string. (default output format is `"%Y-%m-%d"`)\\

```python
data_utils.convert_date_string("2024-12-22", "%Y-%m-%d", "%Y/%m/%d") -> "2024/12/22"
```

### IndexGenerator <a href="#indexgenerator" id="indexgenerator"></a>

```python
custom_idx = data_utils.IndexGenerator(start=0)
```

This class is intended to generate numbers sequentially. A typical use case would be to define field mapping paths with `next()` and `.last` so that the Field Mapping paths are dynamically generated and an _Include For Each_ isn’t necessary.

```python
custom_idx.last → None
next(custom_idx) → 0
custom_idx.last → 0
next(custom_idx) → 1
custom_idx.last → 1
...
```

{% hint style="info" %}
Be sure to `keep()` the instance of IndexGenerator in the relevant hook or an _AttributeError_ exception will be thrown. \
\
e.g:`keep(custom_idx = data_utils.IndexGenerator())`
{% endhint %}

Be aware that Integration Gateway evaluates the _Value_ Column on the Field Mapping table before the _Field_ column which can result in unintended behavior. e.g.:

```
if custom_idx == 11;

Field Path: some.field.path[next(custom_idx)] → some.field.path[12]
Value Column: f”some_value{custom_idx.last}” → “some_value11“
```

<br>
