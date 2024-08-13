After the introductory knowledge graph and transport theorem, today is the first segment of our engineering fundamentals content. Specifically we're going to talk about units and measurement! For engineers, this can be incredibly useful to wrap your head around. Feel comfortable with managing units and measurements--or it will bite you in the derrier.

## Physical Basis

Let's start with the physical basis of what units are. And naturally (sarcasm here), that means starting with relativity and quantum! There are three units that stand heads and tails above the rest, even if we're just focusing on the seven "Base SI" units: length (meters), mass (kilograms), and time (seconds).

Specifically, let's think about "very big" measurements in these units and "very small" measurements.

If you've heard about "Plank units", these represent the fundamental lower limit on what measurements in these dimensions (like Plank length) are useful. At that scale, utility breaks down for engineers--we like things to be deterministic, after all, or they will be impossible to control and design for!

Out at the "very big" end, on the other hand, we need to take relativistic effects into account. This is particularly true for large speeds (rate of length vs. time) and large mass (with implications for spacetime). Engineers may sometimes need to compute relativistic corrections and frames, but not very often.

![three fundamental unit dimensions from physics](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qdtu7cs0241yua3lljh6.png)

So, for the most part, engineers will live in this "happy medium" between quantum and relativistic ends of these three dimensions (length, mass, and time). And units we typically use can even be expressed as exponential combinations of each dimension: `[m]^a * [kg]^b * [s]^c`; for example, speed (in meters per second) might be expressed as `a=1`, `b=0`, and `c=-1`. These exponents are typically integers--but not always.

This has interesting implications for the vector space in which units are embedded. But even avoiding those topics, this can be a very useful way to define and model units in ways that are relevant (and memorable) for engineering purposes. This is true for Base SI as well as non-Base SI and Imperial measurements (though there are some edge cases for sure).

## Base SI

There are various "systems" of units different problems may use, even avoiding the conversion issues. "Base SI", for example, may also be referred to as `mks` for "meters, kilograms, seconds". Other systems you may come across (even while still using Metric) may include `cgs` (for "centimeters, grams, seconds").

Beyond the "core" three units, base SI also includes "dimensions" for current, temperature (of course), quantities, and "luminous intensity" (more on that last one later). That gives us seven fundamental "dimensions" of Base SI units, from which we can express any other measurements or units as a function of these dimensions.

![our seven base SI units](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bh1ibuctn447sg6djsj1.png)

One of the handy things about Base SI units are, there are specific physical references that define each unit. Many Imperial units (or even other Metric units) were originally defined (and still are!) using some "traditional" reference of (for example) what a "stone" might typically weigh. Base SI units have explicit mapping to (for example) the transition of a particular atomic state:

> "The second, symbol s, is the SI unit of time. It is defined by taking the fixed numerical value of the caesium frequency, ∆νCs, the unperturbed ground-state hyperfine transition frequency of the caesium 133 atom, to be 9192631770 when expressed in the unit Hz, which is equal to s−1." [The International System of Units](https://www.bipm.org/documents/20126/41483022/SI-Brochure-9-EN.pdf/2d2b50bf-f2b4-9661-f402-5f9d66e4b507)

This is certainly less ambiguous than a unit like a pint, as we'll see later!

## Operations and Tools

When we talk about calculations with measurements, we understand basic arithmetic operations, but there are additional considerations we need to make when those quantities have units. Let's consider addition.

`a + b = c`

If `a` and `b` have different units, this is not a valid operation! Addition is quite a constrained operation with measurements because the units must be identical, and the units of the result will be identical to the units of the operands.

`a [m] + b [m] = c [m]`

Multiplication, on the other hand, is considerably more interesting because it lets us combine and permute the units we are using. Specifically, the *units of the product are the product of the units*. And for our purposes, this is true for division too since it is just the inverse.

`a [m] / b [s] = c [m/s]`

This can be tricky to handle and model, especially in digital circumstances where the computer may think you are just dealing with numbers. That's why systems like "Pint" (in Python) can be very useful, because they let you extend and constrain the manipulation of specific variables based on what units are assigned to them.

https://pint.readthedocs.io/en/stable/

I also find it's helpful to explicitly label units in your variables when programming engineering calculations. For example, you can use camel-case for the variable name but then indicate units following an underscore. With this approach, integer values can be used to indicate power and the letter `p` (for "per") to indicate a ratio of "before" divided by "after". `speedOfLight_mps`, for example, might store the speed of light in meters per second, whereas `speedOfLight_miphr` might store the speed of light in miles per hour (`mph` would also be acceptable but introduces some ambiguity; I tend to reserve single-letter abbreviations for Base SI, when possible).

## Imperial and Other Edge Cases

While Base SI gives us a great tool to define a "core" of unit dimensions that we can use for converting to and from other systems, those other systems can come with a lot of edge cases. Imperial is the easy one to blame of course (and we'll start there)--but there are some other tricky areas, too.

### Imperial Length

Length is the least tricky of the core dimensions when it comes to Imperial units. Specifically, I find it helps to remember one specific conversion (inches to centimeters) because it has "lossless" precision, and is therefore a great "intermediate" unit regardless of what your source and destination Imperial and Metric units are.

`2.54 [cm] = 1 [in]`

We can use this as the basis for precise conversion between many other units, where the conversion constant would be harder to remember or even of limited or approximate ("lossy") precision.

### Imperial Weight (pounds issues)

There are several "gotchas" to keep in mind when it comes to Imperial weight measurements. We don't have an easily-rememberable constant conversion between (for example) pounds and kilograms. Instead it is typically approximated (if this is sufficient for your calculations) by `2.2 [lbs] = 1 [kg]`. Interestingly, though, there is (as of 1959) an explicit conversion factor--it's just much harder to remember:

`0.45359237 [kg] = 1 [lb]`

Another problem here is the difference between mass and weight. If I tell you I weigh 200 pounds, that's not a measurement of mass! That's a measurement of weight. Weight is a *FORCE*--specifically, the force exerted by gravity on a specific mass. Confusingly, in Imperial, both are measured in "pounds"--so we need to be very careful. "Pounds" should always specify when you are referring to "pounds-mass" (in which case there is a conversion to kilograms) or "pounds-force" (in which case you need to convert to Newtons).

### Imperial Volume (especially pints)

Volume is surprisingly tricky, even though it's just a "cube" of length. Metric volume, of course, is specifically constrained by `1 [cm^3] = 1 [mL]`. (A liter equal in volume to a cube 10 centimeters on each side.) I find it is helpful to use `fl oz` (or "fluid ounces") as a base reference when working with volume measurements in Imperial, for two reasons:

1. It has a relatively straightforward approximate conversion to Metric: `1 [fl oz] ~= 30 [mL]` (this ratio is exact if we are using "US food labeling fluid ounce" standards, but most other conversions will instead use "Imperial fluid ounces" or "US customary fluid ounces").

1. It has a relatively straightforward "ladder" of conversions up to other Imperial units of volume, which will be familiar to anyone with a decent amount of cooking experience:

`8 [fl oz] = 1 [cup]`

`4 [cups] = 1 [quart]`

`4 [quarts] = 1 [gallon]`

The tricky outlier here are "pints". Even putting aside the difference between "Imperial fluid ounces" and "US customary fluid ounces" (I used the latter in the above examples), there is no standard definition of a pint. You can typically assume (unless stated otherwise or within a specific alternative context) that a pint is two cups, or 16 fluid ounces, but be warned!

### Temperature

Temperature is something of an edge case because it's not a proportional conversion if you are using degrees Celsius or degrees Fahrenheit. That relationship is useful to memorize in and of itself, though:

`F = 9/5 * C + 32`

But hopefully you don't have to deal with the non-proportional measurements; rather, you can convert (as soon as possible) into degrees Kelvin or (somewhat less common) degrees Renkin. In the Celsius-Kelvin case, it's helpful to memorize the constant offset (`0 [deg C] = 273.15 [deg K]`). Note that we need to mention what *kind* of degrees we reference! There are different ways to do this (for example, using the special circle character, or changing the spacing, or leaving out `deg` altogether), but I find the approach in the above example useful.

### Radiometry

...and now we get to the fun one. Remember that funky "luminous intensity" unit in Base SI? There's no easy way to tackle the systems of units used in radiometry. But they're important--particularly in thermal analysis, electro-optics, RF calculations, and infrared systems. (They're even useful in computer graphics calculations--e.g., shader calibration--but that's another conversation for another day!)

There are a couple of things about radiometry that make it the devil when it comes to units.

1. If you ever hear "visual magnitude", that's a logarithmic magnitude calibrated off of human perception--that is, what your eyes perceive. So when you see "VM", convert out of it as soon as possible!

1. You'll often see some measurement of luminosity or power emission, but those units are further elaborated when you consider the flux of those measurements through space from a point. This is typically done by normalizing about a solid angle--for example, per "solid radians". In this manner, the measurements (such as power) are consistent regardless of how far away you are from the point.

1. You will also see these units expressed as a unit *per wavelength*, because there will be spectral concerns that describe how a measurement might be distributed across different frequencies. 

Yeah, it can be a pain. Be very, very careful with units when doing radiometric calculations.

![radiometry is the clown car of units](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/han6jzjy0qved9c8by84.jpg)

## Dimensional Analysis and Examples

### Conversions

Let's talk about conversions. We can use our knowledge of unit dimensions and operations to help us solve problems. Let's say we want to convert 100 yards into meters. There will be a "chain" of several conversions that will eventually get us to meters. At each step in the chain, we'll multiple by some fraction that will let us "cancel out" the units from the previous step. We'll use a combination of these steps to then define the arithmetic operation (without mixing up which factors are multiplied or divided!) that lets us get the right answer--and with known precision.

So, to start, remember that it's useful to leverage the lossless precision of the "centimeters/inches" conversion. So let's start by converting yards to inches.

`100 [yd] * (36 [in] / 1 [yd])`

There's a fraction on the right that we can define by asking ourselves, how many inches are the same as how many yards? It is often helpful to first define the quantity in this factor for the smaller unit, because this is often an integer value and therefor more precise. Often, the other value will be 1, and in this manner you'll know (asking which top is the same as which bottom) what factors you are multiplying or dividing by without getting them inverted by mistake.

Now we can apply our lossless conversion from Imperial into Metric:

`100 [yd] * (36 [in] / 1 [yd]) * (2.54 [cm] / 1 [in])`

And finally convert within the known Metric prefixes:

`100 [yd] * (36 [in] / 1 [yd]) * (2.54 [cm] / 1 [in]) * (1 [m] / 100 [cm])`

Now we have a specific arithmetic expression we can evaluate (in this case we even get to cancel out 100 on the top with 100 on the bottom), and verify that the units cancel out to give us our desired answer:

``100 [yd] = 91.44 [m]`

What's more, because none of our conversion factors were approximate we *know* this is a precise conversion.. which is nice.

### Expanding and Consolidating Unit Expressions

In our Base SI system, we didn't see any units defined for force, or power, or acceleration. That is, of course, because those units are a combination of the units in our Base IS list.

Let's consider power, which (in Metric) is measured in Watts. What does that mean (say, for a 100 [W] light bulb)? Well, Watts are a measurement of the rate of energy, or joules per second:

`[W] = [j/s]`

Joules, or energy, are an integrated force--a force applied over some distance. So, a joule is a Newton-meter:

`[W] = [j/s] = [N * m / s]`

A force is a unit of accelerated mass, and acceleration is the second derivative of displacement. SO we can continue expanding our expression of the unit for power:

`[W] = [j/s] = [N * m / s] = [kg * m / s^2 * m / s]`

We're in Base SI now--and of course that means we can consolidate units into a specific expression of power:

`[W] = [kg * m^2 / s^3]`

We can even expand this in terms of our exponential expression, embedded with a logarithmic space of these dimensions:

`[W] = [kg]^1 * [m]^2 * [s]^-3`

### Orbital Period

In orbital mechanics, there is a tricky expression for the period of an orbit, as a function of its orbital properties. It's an angular rate, so we can start with an expression of the period per revolution:

`T / (2 * pi)`

That expression is equal to the area "swept out" by that rate (thanks, Kepler!), but as a square root of some ratio between the "gravitational parameter" (`mu`, a property of the central body, or Earth, in units of `[m^3/s^2]`) and the "semi-major axis" or length of the conic section (`a` in units of `[m]`).

So, we can determine what the inside of that square root is suppsed to be by what expression gives us seconds-squared, since `a^3/mu` has units of `[m^3 / (m^3 / s^2)]`, or `[s^2]`. In other words, we can always back out the expression for orbital period:

`T = 2 * pi * (a^3 / mu)^0.5`

### Gas Constant

Another fun dimensional example is the gas constant. This is a very important quantity (and it's not just my fluids background talking) that pops up often and in very surprising places (and with several different forms or units).

For example, in Base SI units, the gas constant is:

`R = 8.314... [j / deg K / mol]`

... which is a funky beast. What sort of units are those!? Other expressions are just as strange:

`R = 1.9877... [cal / deg K / mol]`

(And note that's calories, not kilo-calories, which is a fun mistake to make when you're trying to think about nutrition labels.)

`R = 8.206...e-5 [m^3 * atm / deg K / mol]`

Why is this so strange? Because `R` represents an invariant of specific ratios in which there are very different units. Recall the ideal gas equation:

`PV = nRT`

So, we know `R` will always have to be a ratio of, on one side, the product of pressure and volume units, and on the other side, the product of particle count (moles) and temperature units. And it's the `PV` units that work out to something particularly interesting in Base SI:

`[N/m^2 * m^3] = [kg * m / s^2 / m^2 * m^3] = [kg * m^2 / s^2]`

## Conclusion

As engineers, we don't really have the luxury of saying "I'm only going to work in Metric" (for example). You'll hear horror stories of, for example, Mars probes that crash because the subcontractor used miles while the JPL system was navigating in kilometers, etc..

Instead, at the end of the day, an engineer needs to be *FLUENT* when it comes to units. You need to know the conversions, and to be able to move around within the space off different unit systems. That's what dimensional analysis gives you--a powerful way to make sure you are holding yourself accountable when it comes to important calculations and approximations.
