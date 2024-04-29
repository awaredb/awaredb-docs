# Constants & Functions

This chapter provides a comprehensive guide to various functions, constants and special attributes that can be used in your formulas. These functions are categorized based on their mathematical properties and uses, making it easier for you to find the right function for your needs.

## Special attributes

They are used to retrieve special values from properties or nodes.

- `children`: Returns all sub-properties or sub-nodes inside of a `path`. Example: `children`.
- `date`: Returns date from a datetime value.
- `time`: Returns time from a datetime value.


## Higher End Functions

All higher end functions are used to transform values from properties values or nodes lists. They can be used as continuation of a path `children.map(x => expression)`.

- `map(x => expression)`: Takes an array of values and applies a transformation to each value. Example: `[1, 2, 3].map(x => x + 1)` returns `[2, 3, 4]`.
- `filter(x => expression)`: Takes an array of values a new array with only values that meet certain criteria. Example: `[1, 2, 3].filter(x => x > 1)` returns `[2, 3]`.
- `reduce(aggregation, current => expression, initial_value (optional))`:  Takes an array of values and applies a transformation to reach a single value. Example: `[1, 2, 3].reduce(total, value => total + value)` returns `6`.


## Mathematical Constants & Functions

### Constants

This chapter provides a list of constants that can be used in your formulas, along with their descriptions and values:

- `pi`: The mathematical constant π (pi), which is the ratio of the circumference of any circle to its diameter. Its approximate value is `3.14159`.
- `e`: The mathematical constant e, which is the base of the natural logarithm. Its approximate value is `2.71828`.
- `nan`: Stands for 'Not a Number'. It's a special floating-point value that represents undefined or unrepresentable numerical results, such as the result of 0/0.
- `inf`: Stands for 'Infinity'. It's a special floating-point value that represents positive infinity.
- `tau`: Tau (τ) is a mathematical constant equal to 2π, the ratio of a circle's circumference to its radius. Its approximate value is `6.28318`.


### Basic Arithmetic

- `abs(x)`: Returns the absolute value of `x`. Example: `abs(-10)` returns `10`.
- `max(x1, x2, ...)`: Returns the maximum value among the arguments. Example: `max(1, 2, 3)` returns `3`.
- `min(x1, x2, ...)`: Returns the minimum value among the arguments. Example: `min(1, 2, 3)` returns `1`.
- `prod(iterable)`: Returns the product of all the numbers in the iterable. Example: `prod([1, 2, 3, 4])` returns `24`.
- `sum(iterable)`: Returns the sum of all the numbers in the iterable. Example: `sum([1, 2, 3, 4])` returns `10`.

### Rounding and Representation

- `ceil(x)`: Returns the smallest integer greater than or equal to `x`. Example: `ceil(2.3)` returns `3`.
- `floor(x)`: Returns the largest integer less than or equal to `x`. Example: `floor(2.3)` returns `2`.
- `round(x, n)`: Rounds `x` to `n` digits from the decimal point. Example: `round(2.3456, 2)` returns `2.35`.

### Exponential and Logarithmic

- `exp(x)`: Returns `e` raised to the power `x`. Example: `exp(1)` returns `2.718281828459045`.
- `expm1(x)`: Returns `e` raised to the power `x` minus 1. More accurate for small `x`. Example: `expm1(1)` returns `1.718281828459045`.
- `log(x, base)`: Returns the logarithm of `x` to the `base`. If `base` is not specified, natural logarithm is assumed. Example: `log(100, 10)` returns `2`.
- `log1p(x)`: Returns the natural logarithm of `1+x`. More accurate for small `x`. Example: `log1p(1)` returns `0.6931471805599453`.
- `log10(x)`: Returns the base-10 logarithm of `x`. Example: `log10(100)` returns `2`.
- `log2(x)`: Returns the base-2 logarithm of `x`. Example: `log2(4)` returns `2`.
- `ln(x)`: Returns the natural logarithm of `x`. Example: `ln(2.718281828459045)` returns `1`.

### Power and Root

- `sqrt(x)`: Returns the square root of `x`. Example: `sqrt(4)` returns `2`.
- `isqrt(x)`: Returns the integer square root of `x`. Example: `isqrt(10)` returns `3`.
- `hypot(x, y)`: Returns the Euclidean norm, `sqrt(x*x + y*y)`. Example: `hypot(3, 4)` returns `5`.

### Factorial and Gamma Functions

- `factorial(x)`: Returns the factorial of `x`. Example: `factorial(5)` returns `120`.
- `gamma(x)`: Returns the Gamma function at `x`. Example: `gamma(5)` returns `24`.
- `lgamma(x)`: Returns the natural logarithm of the absolute value of the Gamma function at `x`. Example: `lgamma(5)` returns `3.178053830347945`.

### Number Property Checks

- `isfinite(x)`: Returns `True` if `x` is neither an infinity nor a NaN (Not a Number), and `False` otherwise. Example: `isfinite(10)` returns `True`.
- `isinf(x)`: Returns `True` if `x` is a positive or negative infinity, and `False` otherwise. Example: `isinf(inf)` returns `True`.
- `isnan(x)`: Returns `True` if `x` is a NaN, and `False` otherwise. Example: `isnan(nan)` returns `True`.

### Error Function
- `erf(x)`: Returns the error function at `x`. Example: `erf(1)` returns `0.8427007929497148`.
- `erfc(x)`: Returns the complementary error function at `x`. Example: `erfc(1)` returns `0.1572992070502851`.

## Trigonometric Functions

### Basic Trigonometry
- `cos(x)`: Returns the cosine of `x` radians. Example: `cos(0)` returns `1`.
- `sin(x)`: Returns the sine of `x` radians. Example: `sin(0)` returns `0`.
- `tan(x)`: Returns the tangent of `x` radians. Example: `tan(0)` returns `0`.

### Inverse Trigonometric
- `acos(x)`: Returns the arc cosine of `x`, in radians. Example: `acos(1)` returns `0.0`.
- `asin(x)`: Returns the arc sine of `x`, in radians. Example: `asin(0)` returns `0.0`.
- `atan(x)`: Returns the arc tangent of `x`, in radians. Example: `atan(1)` returns `0.7853981633974483`.
- `atan2(y, x)`: Returns atan(y / x), in radians. The result is between -pi and pi. Example: `atan2(1, 1)` returns `0.7853981633974483`.

### Hyperbolic Functions
- `cosh(x)`: Returns the hyperbolic cosine of `x`. Example: `cosh(0)` returns `1.0`.
- `sinh(x)`: Returns the hyperbolic sine of `x`. Example: `sinh(0)` returns `0.0`.
- `tanh(x)`: Returns the hyperbolic tangent of `x`. Example: `tanh(0)` returns `0.0`.

### Inverse Hyperbolic
- `acosh(x)`: Returns the inverse hyperbolic cosine of `x`. Example: `acosh(1)` returns `0.0`.
- `asinh(x)`: Returns the inverse hyperbolic sine of `x`. Example: `asinh(0)` returns `0.0`.
- `atanh(x)`: Returns the inverse hyperbolic tangent of `x`. Example: `atanh(0)` returns `0.0`.

## Statistical Functions

### Central Tendency
- `mean(iterable)`: Returns the arithmetic mean (average) of the numbers in the iterable. Example: `mean([1, 2, 3, 4])` returns `2.5`.
- `median(iterable)`: Returns the median (middle value) of the numbers in the iterable. Example: `median([1, 2, 3, 4])` returns `2.5`.
- `mode(iterable)`: Returns the most common data point from the iterable. Example: `mode([1, 1, 2, 3])` returns `1`.
- `multimode(iterable)`: Returns a list of the most common data points. Example: `multimode([1, 1, 2, 2, 3])` returns `[1, 2]`.
- `fmean(iterable)`: Returns the fast, floating point arithmetic mean. Example: `fmean([1, 2, 3, 4])` returns `2.5`.
- `geometric_mean(iterable)`: Returns the geometric mean of the numbers in the iterable. Example: `geometric_mean([1, 2, 3, 4])` returns `2.213363839400643`.
- `harmonic_mean(iterable)`: Returns the harmonic mean of the numbers in the iterable. Example: `harmonic_mean([1, 2, 3, 4])` returns `1.9200000000000004`.

### Dispersion
- `pstdev(iterable)`: Returns the population standard deviation (square root of the population variance). Example: `pstdev([1, 2, 3, 4])` returns `1.118033988749895`.
- `pvariance(iterable)`: Returns the population variance of the numbers in the iterable. Example: `pvariance([1, 2, 3, 4])` returns `1.25`.
- `stdev(iterable)`: Returns the sample standard deviation (square root of the sample variance). Example: `stdev([1, 2, 3, 4])` returns `1.2909944487358056`.
- `variance(iterable)`: Returns the sample variance of the numbers in the iterable. Example: `variance([1, 2, 3, 4])` returns `1.6666666666666667`.

### Position
- `quantiles(iterable, n)`: Returns the `n` equally distributed quantiles of the numbers in the iterable. Example: `quantiles([1, 2, 3, 4], 2)` returns `[2.5]`.
- `median_grouped(iterable)`: Returns the median of grouped continuous data, calculated as the 50th percentile, using interpolation. Example: `median_grouped([52, 52, 53, 54])` returns `52.5`.
- `median_high(iterable)`: Returns the high median of the numbers in the iterable. Example: `median_high([1, 2, 3, 4])` returns `3`.
- `median_low(iterable)`: Returns the low median of the numbers in the iterable. Example: `median_low([1, 2, 3, 4])` returns `2`.

### Correlation
- `correlation(iterableX, iterableY)`: Returns the correlation coefficient between two data sets. Example: `correlation([1, 2, 3, 4], [4, 3, 2, 1])` returns `-1.0`.
- `covariance(iterableX, iterableY)`: Returns the covariance between two data sets. Example: `covariance([1, 2, 3, 4], [4, 3, 2, 1])` returns `-1.6666666666666667`.
