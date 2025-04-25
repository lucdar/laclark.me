+++
title = "BPM Tap Tool"
date = "2025-03-22"
draft = true
+++

[bpm](https://laclark.me/tools/bpm) |
[source on GitHub](https://github.com/lucdar/bpm)

# Usage

This website is a tool that gives a beats-per-minute estimation of the rate of
your inputs. For me, this is mainly relevant for music, but other applications
are possible (e.g. heart rate). I use tools like this when writing charts for
rhythm games or when I'm just curious about the speed of a song.

To use it, tap a key on your keyboard or click (outside the box) to the beat of
whatever it is you are trying to measure. After two inputs, the UI will update
and give you estimations of the rate of your taps in beats per minute. Generally
speaking, more data will result in more accurate estimations.

# Algorithms

Since the inputs are from a human, there will naturally be some variation: some
will be a little early, and others a little late. There are three different
algorithms that are used for computing the estimations from these noisy
measurements, each with their own benefits and shortcomings.

Inputs are collected as a list of timestamps; the first timestamp corresponds to
the first beat (and so on). When reasoning about this problem, it was helpful to
imagine plotting this data on a coordinate plane. Here's an example with some
imaginary data:

<!-- TODO: add image here -->

The x-axis corresponds to the timestamp and the y-axis corresponds to the number
of beats before that measurement (i.e. its index in the list of timestamps).

Recall that $ \text{slope} = \frac{\Delta y}{\Delta x} =
\frac{\text{rise}}{\text{run}} $. In our encoding, a change in y (a "rise")
corresponds to an increase in counted beats, and a change in x (a "run")
corresponds to a change in time. This means slope is a measure of $
\frac{\text{beats}}{\text{time}} $, or beats per minute! In other words, we want
to find a slope that's representative of the data.

## Direct

The most straightforward way of doing this is to look at the endpoints of our
data and compute the slope just from these two points. This is equivalent to
drawing a line connecting the two endpoints and measuring its slope.

<!-- TODO: Image of direct demonstration -->

One issue with this method is that if the start or end are especially noisy
compared to the rest of the data, the resulting BPM estimation could be very far
from the correct value. Though, this noise's effect diminishes as the number of
samples increases.

<!-- TODO: Image of noisy direct (two sample sizes?) -->

The implementation of this algorithm is quite simple and runs in constant time.

```Rust
pub fn direct_count(offsets: &[u64]) -> f64 {
    // get the first and last values
    let start = offsets[0];
    let end = offsets.last().unwrap();
    // calculate the change in time
    let delta = end - start;
    // len - 1 is used so only one of start/end is counted
    let count = (offsets.len() - 1) as f64;
    count * 60_000_f64 / delta as f64
}
```

## Least-Squares Linear Regression

The direct approach above works reasonably well, but we're ignoring a lot of our
data. Instead of just fitting a line to first and last data point, we can find
the line that fits our data the best. The notion of _fitting our data the best_
isn't mathematically precise, and there are many different approaches we could
take.

The underlying model of our data is a linear equation:

$$ y = mx + b $$

$ y $ (the number of beats) increases at a constant rate with respect to $ x $
(the time elapsed). $ m $ is this rate of increase (the slope) and is
proportional to the bpm. $ b $ is the y-intercept and accounts for the input
offset and any constant error, but it's not necessary for computing $ m $ which
is what we're after.

This model can "predict" what our number of beats should be, given a certain
timestamps. It'll probably be a little wrong, but we can quantify this error by
subtracting the actual measurement by the computed measurement and squaring it.

$$ (y_i - (mx_i + b))^2 $$

Why are we squaring the error? We want all the error values to be positive so
when we add them up across all our data points they accumulate instead of
canceling out. Technically, absolute value would also do this, but squaring the
error makes the math _much_ easier.

{% note(clickable=true, header="The Math") %}

test

<!-- TODO: this isn't working :(((( -->

{% end %}

The slope can be computed by the following equation.

Since the data points are processed one-by-one, it would be possible to write an
algorithm that stores the current values for

## Thiel-Sen Estimator

When browsing the Wikipedia page for Linear Regression,

# Design

<!-- TODO: rs1n's Super Metroid guide -->
<!-- https://gamefaqs.gamespot.com/snes/588741-super-metroid/faqs/10114 -->

<!-- terminal aesthetics -->
<!-- leptos, ChatGPT -->

# Deployment
