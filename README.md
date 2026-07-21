<!--

@license Apache-2.0

Copyright (c) 2026 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# incrnanmme

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute a moving [mean error][mean-absolute-error] (ME) incrementally, ignoring `NaN` values.

<section class="intro">

For a window of size `W`, the [mean error][mean-absolute-error] is defined as

<!-- <equation class="equation" label="eq:mean_error" align="center" raw="\mathop{\mathrm{ME}} = \frac{1}{W} \sum_{i=0}^{W-1} (y_i - x_i)" alt="Equation for the mean error."> -->

```math
\mathop{\mathrm{ME}} = \frac{1}{W} \sum_{i=0}^{W-1} (y_i - x_i)
```

<!-- <div class="equation" align="center" data-raw-text="\operatorname{ME} = \frac{1}{W} \sum_{i=0}^{W-1} (y_i - x_i)" data-equation="eq:mean_error">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@634ac3689760e2f57fd51085f387d8dc5bb3b927/lib/node_modules/@stdlib/stats/incr/nanmme/docs/img/equation_mean_error.svg" alt="Equation for the mean error.">
    <br>
</div> -->

<!-- </equation> -->

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import incrnanmme from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmme@deno/mod.js';
```

#### incrnanmme( window )

Returns an accumulator `function` which incrementally computes a moving [mean error][mean-absolute-error], ignoring any `(x, y)` pairs containing `NaN`. The `window` parameter defines the number of values over which to compute the moving [mean error][mean-absolute-error].

```javascript
var accumulator = incrnanmme( 3 );
```

#### accumulator( \[x, y] )

If provided input values `x` and `y`, the accumulator function returns an updated [mean error][mean-absolute-error]. If not provided input values `x` and `y`, the accumulator function returns the current [mean error][mean-absolute-error].

```javascript
var accumulator = incrnanmme( 3 );

var m = accumulator();
// returns null

// Fill the window...
m = accumulator( 2.0, 3.0 ); // [(2.0, 3.0)]
// returns 1.0

m = accumulator( -5.0, 2.0 ); // [(2.0, 3.0), (-5.0, 2.0)]
// returns 4.0

m = accumulator( 3.0, NaN ); // NaN pair ignored: [(2.0, 3.0), (-5.0, 2.0)]
// returns 4.0

// Window begins sliding...
m = accumulator( -7.0, 3.0 ); // [(2.0, 3.0), (-5.0, 2.0), (-7.0, 3.0)]
// returns 6.0

m = accumulator( -5.0, -3.0 ); // [(-5.0, 2.0), (-7.0, 3.0), (-5.0, -3.0)]
// returns ~6.33

m = accumulator();
// returns ~6.33
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Input values are **not** type checked. Any `(x, y)` pair containing `NaN` is ignored and does **not** update the window.
-   As `W` (x,y) pairs are needed to fill the window buffer, the first `W-1` returned values are calculated from smaller sample sizes. Until the window is full, each returned value is calculated from all provided values.
-   Be careful when interpreting the [mean error][mean-absolute-error] as errors can cancel. This stated, that errors can cancel makes the [mean error][mean-absolute-error] suitable for measuring the bias in forecasts.
-   **Warning**: the [mean error][mean-absolute-error] is scale-dependent and, thus, the measure should **not** be used to make comparisons between datasets having different scales.

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import randu from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-randu@deno/mod.js';
import incrnanmme from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmme@deno/mod.js';

var accumulator;
var v1;
var v2;
var i;

// Initialize an accumulator:
accumulator = incrnanmme( 5 );

// For each simulated datum, update the moving mean error...
for ( i = 0; i < 100; i++ ) {
    v1 = ( randu()*100.0 ) - 50.0;
    v2 = ( randu()*100.0 ) - 50.0;
    accumulator( v1, v2 );
}
console.log( accumulator() );
```

</section>

<!-- /.examples -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-incr-nanmme.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-incr-nanmme

[test-image]: https://github.com/stdlib-js/stats-incr-nanmme/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/stats-incr-nanmme/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-incr-nanmme/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-incr-nanmme?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-incr-nanmme.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-incr-nanmme/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-incr-nanmme/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-incr-nanmme/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-incr-nanmme/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-incr-nanmme/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-incr-nanmme/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-incr-nanmme/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-incr-nanmme/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-incr-nanmme/main/LICENSE

[mean-absolute-error]: https://en.wikipedia.org/wiki/Mean_absolute_error

<!-- <related-links> -->

<!-- </related-links> -->

</section>

<!-- /.links -->

