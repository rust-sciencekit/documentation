# Polynomials

This chapter covers the `Polynomial` type and how to use it.

The `Polynomial` type is defined as a struct containing a vector of real coefficients:

``` rust, noplayground
    struct Polynomial {
    coefficients: Vec<f64>
}
```

where the first -- or left-most -- coefficient corresponds to the lowest power of \\(x \\). In this context, a polynomial of degree \\(n \\) has the form:

\\[ 
    p(x) = a_0 + a_1 x + a_2 x^2 + ... + a_n x^n
\\]

with \\(a_n \\) is the n-th element of the `coefficients` vector.

## Creating a polynomial

Polynomials can be created using the `sciencekit::polynomial!` macro. For example, the function \\( f(x) = 3 + 2x + x^2 \\) can be created like so:

``` rust, noplayground
    let p: Polynomial = sciencekit::polynomial![3, 2, 1];
    println!("{p}");
```

```
    x² + 2x + 3
```

> Note: This macro works with any kind of number type. However, this doesn't affect the way the polynomial stores its coefficients because everything is converted as `f64` under the hood.

The code snippet above is equivalent to a call of the `Polynomial::from_coefficients` function. The only difference is that the coefficients must be of type `Vec<f64>`.

``` rust, noplayground
    let p = Polynomial::from_coefficients(vec![3.0, 2.0, 1.0]);
    println!("{p}");
```

```
    x² + 2x + 3
```

A default polynomial can also be made using the `Polynomial::new` function. Since the coefficients vector can't be empty, the default polynomial is set to \\( f(x) = 0 \\).

``` rust, noplayground
    let p = Polynomial::new();
    println!("{p}");
```

```
    0
```

Polynomials can also be created from a collection of points. The `from_points` method takes a collection of n points in the form `Vec<(f64, f64)>` and uses the Newton interpolation method to return a polynomial of degree `n-1` that pass through all the points.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = Polynomial::from_points(vec![(-1.0, 0.0), (0.0, 1.0), (1.0, 0.0)]).unwrap();

    println!("p(x) = {}\n", p);
}
```

```
p(x) = -x² + 1
```

Similarly, once can create a polynomial from a collection of roots.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = Polynomial::from_roots(vec![-1.0, 0.0, 1.0]);

    println!("p(x) = {}\n", p);
}
```

```
p(x) = x³ - x
```

## Evaluation of a polynomial

A polynomial can be evaluated for a given \\( x \\) of type `f64` using the `evaluate` function and of type `Complex` using the `evaluate_complex` function.

``` rust, noplayground
    use sciencekit::polynomial::*;
    use sciencekit::complex::*;

    fn main() {
        let p = sciencekit::polynomial![1, 2, 3];
        println!("p(x) = {p}");
        println!("p(3) = {}", p.evaluate(3.0));
        println!("p(1 + 2i) = {}", p.evaluate_complex(Complex { re: 1.0, im: 2.0}));
    }
```

```
    p(x) = 3x² + 2x + 1
    p(3) = 34
    p(1 + 2i) = -6 + 16i
```

## The degree of a polynomial

The degree of a polynomial is given by the `degree` method.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let degree_p: usize = p.degree();

    println!("{p} is of degree {degree_p}");
}
```

```
3x² + 2x + 1 is of degree 2
```

> Note: Technically speaking, the degree of a 0 polynomial is not clearly defined. The `degree` method could return an `Option<usize>` to be mathematicaly correct but that would add a lot of complexity in the code for such a simple function. So it returns 0 instead.  

## The roots of a polynomial

The `real_roots` method returns the real roots of any polynomial in ascending order. Analytic solutions is given for polynomial of degree 1, 2, or 3. For polynomial of degrees higher than 3, the roots are computing using the eigenvalues of the companion matrix.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = Polynomial::from_roots(vec![-1.0, 1.0]);

    println!("p(x) = {}\n", p);

    let roots: Vec<f64> = p.real_roots();

    println!("The roots of p are {:?}", roots);
}
```

```
p(x) = x² - 1

The roots of p are [-1.0, 1.0]
```



## Integral and derivative

The `differenciate` method transforms a polynomial into its derivative. The transformation is done in-place.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];
    println!("f(x) = {p}");

    p.differentiate();
    println!("f'(x) = {p}");
}
```

```
f(x) = 3x² + 2x + 1
f'(x) = 6x + 2
```

The `derivative` method acts as a non-mutating version of `differenciate`, it does the same thing but returns the derivative as a new polynomial.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = p.derivative();

    println!("f(x) = {p}");
    println!("f'(x) = {q}");
}
```

```
f(x) = 3x² + 2x + 1
f'(x) = 6x + 2
```

The `integrate` method transforms a polynomial into its indefinite integral with a given constant. The transformation is done in-place.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];
    println!("f(x) = {p}");

    p.integrate(2.0);
    println!("∫f(x)dx = {p}");
}
```

```
f(x) = 3x² + 2x + 1
∫f(x)dx = x³ + x² + x + 2
```

The `integral` method acts as a non-mutating version of `integrate`, it does the same thing but returns the derivative as a new polynomial.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = p.integral(2.0);

    println!("f(x) = {p}");
    println!("∫f(x)dx = {q}");
}
```

```
f(x) = 3x² + 2x + 1
∫f(x)dx  = x³ + x² + x + 2
```

## Mutating arithmetic methods

basic Arithmetic operations between polynomials and between `Polynomial` and `f64` are allowed. All of them are available in the form of mutating methods.

* ``` rust
  add_polynomial(&mut self, other: &Polynomial)
  ```

    Adds another polynomial to self.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = sciencekit::polynomial![2, 8, 1, 6];

    println!("f(x) = {p}");
    println!("q(x) = {q}");
    
    println!("Adding q to p");
    p.add_polynomial(&q);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    q(x) = 6x³ + x² + 8x + 2
    Adding q to p
    p(x) = 6x³ + 4x² + 10x + 3
    ```

* ``` rust
  add_real(&mut self, x: f64)
  ```

    Adds a real number to self.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];

    println!("f(x) = {p}");
    
    println!("Adding 1 to p");
    p.add_real(1.0);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    Adding 1 to p
    p(x) = 3x² + 2x + 2
    ```

* ``` rust
  sub_polynomial(&mut self, other: &Polynomial)
  ```

    Substract another polynomial from self.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = sciencekit::polynomial![2, 8, 1, 6];

    println!("f(x) = {p}");
    println!("q(x) = {q}");
    
    println!("Substracting q from p");
    p.sub_polynomial(&q);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    q(x) = 6x³ + x² + 8x + 2
    Substracting q from p
    p(x) = -6x³ + 2x² - 6x - 1
    ```

* ``` rust
  sub_real(&mut self, x: f64)
  ```

    Substract a real number from self.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];

    println!("f(x) = {p}");
    
    println!("Adding 1 to p");
    p.sub_real(1.0);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    Substracting 1 from p
    p(x) = 3x² + 2x
    ```

* ``` rust
  mul_by_polynomial(&mut self, other: &Polynomial)
  ```

    Multiply self by another polynomial.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = sciencekit::polynomial![2, 8, 1, 6];

    println!("f(x) = {p}");
    println!("q(x) = {q}");
    
    println!("Multiplying p by q");
    p.mul_by_polynomial(&q);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    q(x) = 6x³ + x² + 8x + 2
    Multiplying p by q
    p(x) = 18x⁵ + 15x⁴ + 32x³ + 23x² + 12x + 2
    ```

* ``` rust
  mul_by_real(&mut self, x: f64)
  ```

    Multiply self by a real number.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];

    println!("f(x) = {p}");
    
    println!("Multiplying p by 2");
    p.mul_by_real(2.0);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    Multiplying p by 2
    p(x) = 6x² + 4x + 2
    ```

* ``` rust
  div_by_real(&mut self, x: f64)
  ```

    Divide self by a real number.

    ``` rust, noplayground
    let mut p: Polynomial = sciencekit::polynomial![1, 2, 3];

    println!("f(x) = {p}");
    
    println!("Dividing p by 2");
    p.div_by_real(2.0);

    println!("p(x) = {p}");
    ```

    ```
    f(x) = 3x² + 2x + 1
    Dividing p by 2
    p(x) = 1.5x² + x + 0.5
    ```

> Note: The division between two polynomials is not defined because it generaly doesn't returns another polynomial.

## Non-mutating arithmetic operators

Basic arithmetic operators (`+, -, *, /`) are also provided. All those operations must be used between references of polynomials `&Polynomial` to avoid them to be moved.

The following code, for instance, doesn't compile:

``` rust, compile_fail, noplayground 
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = sciencekit::polynomial![2, 3, 4, 5];

    println!("p(x) = {}", p);
    println!("q(x) = {}", q);

    println!("p + q = {}", p + q);
    println!("p - q = {}", &p - &q);  
}
```

```
error[E0382]: borrow of moved value: `p`
...
10 |     println!("p + q = {}", p + q);
   |                            ----- `p` moved due to usage in operator
11 |     println!("p - q = {}", &p - &q);  
   |                            ^^ value borrowed here after move
   |
...
error[E0382]: borrow of moved value: `q`
...
10 |     println!("p + q = {}", p + q);
   |                                - value moved here
11 |     println!("p - q = {}", &p - &q);  
   |                                 ^^ value borrowed here after move
   |
...
```

The following code, however, works just fine.

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];
    let q: Polynomial = sciencekit::polynomial![2, 3, 4, 5];

    println!("p(x) = {}", p);
    println!("q(x) = {}\n", q);

    println!("p + q = {}", &p + &q);
    println!("p - q = {}", &p - &q);
    println!("p * q = {}", &p * &q);  
}
```

```
p(x) = 3x² + 2x + 1
q(x) = 5x³ + 4x² + 3x + 2

p + q = 5x³ + 7x² + 5x + 3
p - q = -5x³ - x² - x - 1
p * q = 15x⁵ + 22x⁴ + 22x³ + 16x² + 7x + 2
```

> Note: The division between two polynomials is not defined because it generaly doesn't returns another polynomial.

The same operations (with divisions this time) are defined between polynomials and the `f64` type. 

``` rust, noplayground
use sciencekit::polynomial::*;

fn main() {
    let p: Polynomial = sciencekit::polynomial![1, 2, 3];

    println!("p(x) = {}\n", p);

    println!("p + 2.0 = {}", &p + 2.0);
    println!("p - 2.0 = {}", &p - 2.0);
    println!("p * 2.0 = {}", &p * 2.0);
    println!("p / 2.0 = {}", &p / 2.0);
}
```

```
p(x) = 3x² + 2x + 1

p + 2.0 = 3x² + 2x + 3
p - 2.0 = 3x² + 2x - 1
p * 2.0 = 6x² + 4x + 2
p / 2.0 = 1.5x² + x + 0.5
```