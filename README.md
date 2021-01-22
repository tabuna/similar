# <img src=".github/logo.svg?sanitize=true" width="24" height="24" alt="Similar PHP"> Similar

![Unit tests](https://github.com/tabuna/similar/workflows/Unit%20tests/badge.svg)

This is an elementary library for working on identifying similar strings in PHP without using machine learning. It allows you to get groups of one topic from the transferred set of sentences. For example, combine news headlines from different publications, as Google News does. Want to see real use? No problem, this is just used by the Russian news aggregator https://tsmi.live

## Installation

Run this at the command line:

```php
$ composer require tabuna/similar
```

## Usage

You are passing a closure function as input that determines if two strings and a set of strings are similar:

```php
use Tabuna\Similar\Similar;

$similar = new Similar(function (string $a, string $b) {
    similar_text($a, $b, $copy);

    return 51 < $copy;
});

$similar->findOut([
    'Elon Musk gets mixed COVID-19 test results as SpaceX launches astronauts to the ISS',
    'Elon Musk may have Covid-19, should quarantine during SpaceX astronaut launch Sunday',

    // Superfluous word
    'Can Trump win with ‘fantasy’ electors bid? State GOP says no',
]);
```

As a result, there will be only one group containing headers:

```php
'Elon Musk gets mixed COVID-19 test results as SpaceX launches astronauts to the ISS',
'Elon Musk may have Covid-19, should quarantine during SpaceX astronaut launch Sunday',
```

## Keys

The input array stores its keys so that you can do additional processing:

```php
$similar->findOut([
    'kos' => "Trump acknowledges Biden's win in latest tweet",
    'foo' => 'Elon Musk gets mixed COVID-19 test results as SpaceX launches astronauts to the ISS',
    'baz' => 'Trump says Biden won but again refuses to concede',
    'bar' => 'Elon Musk may have Covid-19, should quarantine during SpaceX astronaut launch Sunday',
]);
```

The result will be two groups:

```php
[
    'foo' => 'Elon Musk gets mixed COVID-19 test results as SpaceX launches astronauts to the ISS',
    'bar' => 'Elon Musk may have Covid-19, should quarantine during SpaceX astronaut launch Sunday',
],
[
    'baz' => 'Trump says Biden won but again refuses to concede',
    'kos' => "Trump acknowledges Biden's win in latest tweet",
],
```

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
