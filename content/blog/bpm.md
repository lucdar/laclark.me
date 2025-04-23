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
\frac{\text{rise}}{\text{run}} $. In our encoding, a "rise" corresponds to an
increase in counted beats, and a "run" corresponds to a change in time. This
means slope is a measure of $ \frac{\text{beats}}{\text{time}} $. In other
words, we want to find a slope (or _steepness_) that's representative of the
data.

## Direct

The most straightforward way of doing this is to look at the endpoints of our
data and compute the slope just from these two points. This is equivalent to
drawing a line connecting the two endpoints and measuring its slope.

<!-- TODO: Image of direct demonstration -->

One issue with this method is that if the start or end are especially noisy
compared to the rest of the data, the resulting BPM estimation could be very
far from the correct value. Though, this noise's effect diminishes as the number of
samples increases.

<!-- TODO: Image of noisy direct (two sample sizes?) -->

<!-- TODO: Code and Complexity -->

## Simple Linear Regression

## Thiel-Sen Estimator

When browsing the Wikipedia page for Linear Regression,

# Design

<!-- TODO: rs1n's Super Metroid guide -->
<!-- https://gamefaqs.gamespot.com/snes/588741-super-metroid/faqs/10114 -->

<!-- terminal aesthetics -->
<!-- leptos, ChatGPT -->

# Deployment
