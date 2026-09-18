# Complex numbers

In this chapter, we give a detailed description of how to work with the complex number data type. Complex numbers are represented by the `Complex` type which is defined as:

``` rust, noplayground
struct Complex {
    re: f64,
    im: f64
}
```

where `re` is the real part and `im` the imaginary part.

## Creating a new complex number

To create a new complex number, we can call the `Complex::new` function.

``` rust, noplayground
use sciencekit::complex::*;

fn main() {
    let z = Complex::new();
    println!("z = {z}");
}
```

```
z = 0 + 0i
```

More specific complex numbers can also be created from cartesian coordinates using `Complex::from_cartesian` and from polar coordinates using `Complex::from_polar`.

``` rust, noplayground
use std::f64::consts::FRAC_PI_2;
use sciencekit::complex::*;

fn main() {
    let z1 = Complex::from_cartesian(2.0, 1.0);
    let z2 = Complex::from_polar(2.0, FRAC_PI_2);
    
    println!("z1 = {z1}");
    println!("z2 = {z2}");
}
```

```
z1 = 2 + 1i
z2 = 0.00000000000000012246467991473532 + 2i
```

Additionaly, a complex number can be created by directly creating a `struct` with a `re` and an `im` field.

``` rust, noplayground
use sciencekit::complex::*;

fn main() {
    let z = Complex { re: 1.0, im: 3.0 };
    println!("z = {z}");
}
```

```
z = 1 + 3i
```

Both real and imaginary parts of a complex number can be accessed using the `.re` and `.im` syntax. Mutable complex numbers can also be modified directly using the same method.

``` rust,noplayground
use sciencekit::complex::*;

fn main() {
    let mut z = Complex::from_cartesian(2.0, 1.0);

    println!("The real part of z is {}", z.re);

    z.re = 4.0;

    println!("Wait, now the real part of z is {}!", z.re);
}
```

```
The real part of z is 2
Wait, now the real part of z is 4!
```

## Complex arithmetic

Basic arithmetic operations (Addition, Substraction, multiplication, division, and negation) are fully implemented.

``` rust, noplayground
use sciencekit::complex::*;

fn main() {
    let z1 = Complex { re: 1.0, im: 2.0 };
    let z2 = Complex { re: -2.0, im: 1.0 };

    println!("z1 = {z1}");
    println!("z2 = {z2}");

    println!("z1 + z2 = {}", z1 + z2);
    println!("z1 - z2 = {}", z1 - z2);
    println!("z1 * z2 = {}", z1 * z2);
    println!("z1 / z2 = {}", z1 / z2);

    println!("-z1 = {}", -z1);
}
```

```
z1 = 1 + 2i
z2 = -2 + 1i
z1 + z2 = -1 + 3i
z1 - z2 = 3 + 1i
z1 * z2 = -4 + -3i
z1 / z2 = 0 + -1i
-z1 = -1 + -2i
```

The same arithmetic operations are also defined with the `f64` type.

> Note: Since the library is still on its early stage of development and because `f64` is assumed to be the most used type in scientific computing, only the `f64` type is defined for complex numbers. In the futur, I don't see any reason to not make the complex type generic over any numerical data type (except that it requires a lot more work) so this is definitely going to change. However, I don't expect this to introduce breaking changes so this documentation should't be affected.

``` rust, noplayground
use sciencekit::complex::*;

fn main() {
    let z = Complex { re: 2.0, im: 3.0 };

    println!("z + 2 = {}", z + 2.0);
    println!("z - 2 = {}", z - 2.0);
    println!("z * 2 = {}\n", z * 2.0);

    println!("z / 2 = {}", z / 2.0);
    println!("2 / z = {}", 2.0 / z);
}
```

```
z + 2 = 4 + 3i
z - 2 = 0 + 3i
z * 2 = 4 + 6i

z / 2 = 1 + 1.5i
2 / z = 0.3076923076923077 + -0.46153846153846156i
```

## Elementary complex functions

The following functions are all implemented as part of the `Complex` type and can all be called using the `z.<METHOD>` syntax.

* `magnitude()`

    The magnitude of a complex number. Defined as \\( |a+bi| = \sqrt{a^2 + b^2} \\)
    
    ```rust, noplayground
    let z = Complex { re: 3.0, im: 4.0 };
    let m = z.magnitude();  // m = 5.0
    ```

* `magnitude_squared()`

    The squared magnitude of a complex number. Defined as \\( |a+bi| = a^2 + b^2 \\)
    
    ```rust, noplayground
    let z = Complex { re: 3.0, im: 4.0 };
    let m = z.magnitude_squared();  // m = 25.0
    ```
    
* `argument()`

    The argument of a complex number. Where \\( -\pi < arg(z) <= \pi \\)
    
    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    let arg = z.argument();  // arg = 0.785...
    ```
    
* `inverse()`

    The inverse of a complex number defined as \\( \frac{1}{z} \\).
    
    ```rust, noplayground
    let z = Complex { re: 2.0, im: 3.0 };
    println!("1 / ({z}) = {}", z.inverse());
    ```
    
    ```
    1 / (2 + 3i) = 0.15384615384615385 + -0.23076923076923078i
    ```
    
* `conjugate()`

    The complex comjugate a complex number \\( \overline{a+bi} = a-bi \\)
    
    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("z* = {}", z.conjugate());
    ```
    
    ```
    z* = 1 - 1i
    ```
    
* `sqrt()`

    The square root a complex number
    
    ```rust, noplayground
    let z = Complex { re: 2.0, im: 3.0 };
    println!("The square root of {z} = {}", z.sqrt());
    ```
    
    ```
    The square root of 2 + 3i = 1.6741492280355401 + 0.895977476129838i
    ```
    
* `powi()`

    The complex number raised to an integer exponent
    
    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("({z})² = {}", z.powi(2));
    ```
    
    ```
    (1 + 1i)² = 0.00000000000000017319121124709873 + 2.8284271247461907i
    ```
    
* `powf()`

    The complex number raised to a real exponent
    
    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("({z})² = {}", z.powf(2.0));
    ```
    
    ```
    (1 + 1i)² = 0.00000000000000017319121124709873 + 2.8284271247461907i
    ```
    
* `powc()`

    The complex number raised to a complex exponent
    
    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    let exponent = Complex { re: 2.0, im: 1.0 };
    println!("{z} exponent {exponent} is equal to {}", z.powc(exponent));
    ```
    
    ```
    1 + 1i exponent 2 + 1i is equal to -0.30974350492849356 + 0.8576580125887358i
    ```
    
* `exp()`

    The exponential of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("exp({z}) = {}", z.exp());
    ```

    ```
    exp(1 + 1i) = 1.4686939399158851 + 2.2873552871788423i
    ```

* `ln()`

    The natural logarithm of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("ln({z}) = {}", z.ln());
    ```

    ```
    ln(1 + 1i) = 0.3465735902799727 + 0.7853981633974483i
    ```
    
* `log()`

    The base-b logarithm of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("log2({z}) = {}", z.log(2.0));
    ```

    ```
    log2(1 + 1i) = 0.5000000000000001 + 1.1330900354567985i
    ```

## Complex trigonometric functions

* `sin()`

    The complex sine of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("sin({z}) = {}", z.sin());
    ```

    ```
    sin(1 + 1i) = 1.2984575814159773 + 0.6349639147847361i
    ```

* `cos()`

    The complex cosine of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("cos({z}) = {}", z.cos());
    ```

    ```
    cos(1 + 1i) = 0.8337300251311491 + -0.9888977057628651i
    ```

* `tan()`

    The complex tangent of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("tan({z}) = {}", z.tan());
    ```

    ```
    tan(1 + 1i) = 0.2717525853195117 + 1.0839233273386946i
    ```

* `sec()`

    The complex secant of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("sec({z}) = {}", z.sec());
    ```

    ```
    sec(1 + 1i) = 0.4983370305551868 + 0.591083841721045i
    ```

* `csc()`

    The complex cosecant of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("csc({z}) = {}", z.csc());
    ```

    ```
    csc(1 + 1i) = 0.6215180171704283 + -0.3039310016284264i
    ```

* `cot()`

    The complex cotangent of a complex number

    ```rust, noplayground
    let z = Complex { re: 1.0, im: 1.0 };
    println!("cot({z}) = {}", z.cot());
    ```

    ```
    cot(1 + 1i) = 0.21762156185440265 + -0.8680141428959249i
    ```

