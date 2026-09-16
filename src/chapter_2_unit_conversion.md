# Unit conversion

The functions described in this chapter provide support for physical unit conversion. All of these functions have the same structure, the only difference between two functions is the quantity that needs to be converted (time, mass, length, ...).

Each function takes 4 arguments:

- The `f64` value we want to convert
- The current unit of the value
- The `f64` exponent of the unit. For example, a surface in `m⁻²` will have an exponent of `2`
- The unit we want the value to be converted in

## Example

A unit conversion function will take the generic form

``` rust, noplayground
sciencekit::units::convert_<QUANTITY>(value, old_unit, exponent, new_unit);
```

For example, the following code snippet converts the Rydberg constant from `m⁻¹` to `nm⁻¹`:

``` rust, noplayground
use sciencekit::constants::f64::RYDBERG_CONSTANT;
use sciencekit::units;

fn main() {
    let rydberg_meters: f64 = RYDBERG_CONSTANT;
    let rydberg_nanometers: f64 = units::convert_length(rydberg_meters, units::Length::Meter, -1.0, units::Length::Nanometer);

    println!("The Rydberg constant is equal to {rydberg_meters} m⁻¹ or {rydberg_nanometers} nm⁻¹!");
}
```

```
The Rydberg constant is equal to 10973731.568157 m⁻¹ or 0.010973731568157001 nm⁻¹!
```

More complex units can also be converted by converting each unit one by one. The following code snippet converts 1 dyne (or 1 g.cm.s⁻²) in Newtons (kg.m.s⁻²):

``` rust, noplayground
use sciencekit::units;

fn main() {
    // 1 dyn = 1 g.cm.s⁻²
    let value_dyne: f64 = 1.0;

    // 1 N = 1 kg.m.s⁻²
    let mut value_newton = units::convert_mass(value_dyne, units::Mass::Gram, 1.0, units::Mass::Kilogram);
    value_newton = units::convert_length(value_newton, units::Length::Centimeter, 1.0, units::Length::Meter);

    println!("{value_dyne} dyn is equal to {value_newton} N!");
}
```

```
1 dyn is equal to 0.00001 N!
```

## Time conversion

Time conversion can be performed using the `sciencekit::units::convert_time()` function. The current unit as well as the target unit must be given to the function using the `Time` enum data type. The following units are available for conversion:

- `Picosecond`
- `Nanosecond`
- `Microsecond`
- `Millisecond`
- `Second`
- `Kilosecond`
- `Minute`
- `Hour`
- `Day`
- `SideralDay`
- `Week`
- `Year`
- `TropicalYear`
- `SideralYear`

The following example converts 1800 seconds in hours.

``` rust, noplayground
use sciencekit::units;

fn main() {
    let time_in_seconds: f64 = 1800.0;
    let time_in_hours: f64 = units::convert_time(time_in_seconds, units::Time::Second, 1.0, units::Time::Hour);

    println!("{time_in_seconds} seconds is equal to {time_in_hours} hours!");
}
```

```
1800 seconds is equal to 0.5 hours!
```

## Length conversion

Length conversion can be performed using the `sciencekit::units::convert_length()` function. The current unit as well as the target unit must be given to the function using the `Length` enum data type. The following units are available for conversion:

- `Kilometer`
- `Meter`
- `Centimeter`
- `Millimeter`
- `Micrometer`
- `Nanometer`
- `Angstrom`
- `Miles`
- `Yard`
- `Feet`
- `Inch`
- `NauticalMiles`

The following example converts 150 m² in ft².

``` rust, noplayground
use sciencekit::units;

fn main() {
    let square_meters: f64 = 150.0;
    let square_foot: f64 = units::convert_length(square_meters, units::Length::Meter, 2.0, units::Length::Feet);

    println!("{square_meters} m² is equal to {square_foot} ft²!");
}
```

```
150 m² is equal to 1614.5865625064582 ft²!
```

## Energy conversion

Energy conversion can be performed using the `sciencekit::units::convert_energy()` function. The current unit as well as the target unit must be given to the function using the `Energy` enum data type. The following units are available for conversion:

- `Joule`
- `Kilojoule`
- `GramCalorie`
- `Kilocalorie`
- `WattHour`
- `KilowattHour`
- `Electronvolt`
- `BritishThermalUnit`
- `USTherm`
- `FootPound`

The following example converts 100 Joules in eV.

``` rust, noplayground
use sciencekit::units;

fn main() {
    let joules: f64 = 100.0;
    let electron_volt: f64 = units::convert_energy(joules, units::Energy::Joule, 1.0, units::Energy::Electronvolt);

    println!("{joules} J is equal to {electron_volt} eV!");
}
```

```
100 J is equal to 624141805018100100000 eV!
```

## Mass conversion

Mass conversion can be performed using the `sciencekit::units::convert_mass()` function. The current unit as well as the target unit must be given to the function using the `Mass` enum data type. The following units are available for conversion:

- `Tonne`
- `Kilogram`
- `Gram`
- `Milligram`
- `Microgram`
- `ImperialTon`
- `USTon`
- `Stone`
- `Pound`
- `Ounce`

The following example converts 100 grams in pounds.

``` rust, noplayground
use sciencekit::units;

fn main() {
    let gram: f64 = 100.0;
    let pound: f64 = units::convert_mass(gram, units::Mass::Gram, 1.0, units::Mass::Pound);

    println!("{gram} g is equal to {pound} pound!");
}
```

```
100 g is equal to 0.22046219825084087 pound!
```

## Angle conversion

Angle conversion can be performed using the `sciencekit::units::convert_angle()` function. The current unit as well as the target unit must be given to the function using the `Angle` enum data type. The following units are available for conversion:

- `Turn`
- `Arcsecond`
- `Arcminute`
- `Degree`
- `Gradian`
- `Radian`
- `Milliradian`

The following example converts π radians in degrees.

``` rust, noplayground
use sciencekit::units;

fn main() {
    let angle_radian: f64 = std::f64::consts::PI;
    let angle_degree: f64 = units::convert_angle(angle_radian, units::Angle::Radian, 1.0, units::Angle::Degree);

    println!("{angle_radian} radians is equal to {angle_degree} degrees!");
}
```

```
3.141592653589793 radians is equal to 180 degrees!
```

## Temperature conversion

Temperature conversion can be performed using the `sciencekit::units::convert_temperature()` function. The current unit as well as the target unit must be given to the function using the `Temperature` enum data type. The following units are available for conversion:

- `Celsius`
- `Fahrenheit`
- `Kelvin`

The following example converts 32 Celsius in Fahrenheit.

``` rust, noplayground
use sciencekit::units;

fn main() {
    let celsius: f64 = 32.0;
    let fahrenheit: f64 = units::convert_temperature(celsius, units::Temperature::Celsius, 1.0, units::Temperature::Fahrenheit);

    println!("{celsius} Celsius is equal to {fahrenheit} Fahrenheit!");
}
```

```
32 Celsius is equal to 89.6 Fahrenheit!
```