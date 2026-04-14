# GW2 Fractal Instabilties

This work is licensed under [![CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/).

## How to use

There's basically two options at the moment:

- Use [Aleeva](https://aleeva.io)'s API. Documentation available [here](https://api.aleeva.io/api-doc.html).
- Integrate the data in this repository directly in your project. This repository contains 4 JSON files:
  - **`instabilities.json`**: Contains the instabilities for a given fractal on a given day.  
    The `instabilities` object contains the fractal level as key and the corresponding instabilities as values
    - e.g. `data["instabilities"]["76"][0]` gives the instabilities for level 76 on `????-01-01`.
  - **`dailies.json`**: Contains the daily fractals on a given day.
  - **`recommended.json`**: Contains the recommended fractals on a given day.
  - **`fractals.json`**: Maps the fractal scale to the corresponding fractal and the required AR.

### How to get the index of the current day

The fractals are on a 15 day rotation. To get the current index, you have to calculate the day of year index (0-365) modulo 15.

> [!WARNING]  
> This index is the same for both leap and non leap years. Notably this will skip the index value 59 (February 29) in non leap years.

### But I don't want to write my own bot!

- [Aleeva](https://aleeva.io)
- [GW2Bot](https://gw2bot.info/) ([repository](https://github.com/Maselkov/GW2Bot))
- [Discretize Discord Instability Bot](https://github.com/discretize/discretize-discord-bot-instabilities)

### Where else can I find the data?

- [@a727891](https://github.com/a727891)'s [Clears Tracker](https://github.com/a727891/BlishHud-Raid-Clears) [Blish Hud module](https://blishhud.com/modules/?module=Soeed.RaidClears)
- [@darthmaim](https://github.com/darthmaim)'s [GW2Treasures.com](https://gw2treasures.com/fractals) ([repository](https://github.com/GW2Treasures/gw2treasures.com))

## The format of the data

Here's a commented TypeScript interface.

```ts
// looks funny but that's how you can get 26-100 as strings
type LowerLevels = `6` | `7` | `8` | `9`;
type UpperLevels = `0` | `1` | `2` | `3` | `4` | `5` | LowerLevels;
type FractalsT2 =
  | `2${LowerLevels}`
  | `3${UpperLevels}`
  | `4${UpperLevels}`
  | `50`; // 26 - 50
type FractalsT3 =
  | Exclude<`5${UpperLevels}`, `50`>
  | `6${UpperLevels}`
  | `7${0 | 1 | 2 | 3 | 4 | 5}`; // 51 - 75
type FractalsT4 =
  | `7${LowerLevels}`
  | `8${UpperLevels}`
  | `9${UpperLevels}`
  | `100`; // 76 - 100

type Tier = `T2` | `T3` | `T4`;
type FractalLevel<N extends Tier> = N extends `T2`
  ? FractalsT2
  : N extends `T3`
    ? FractalsT3
    : FractalsT4;

// list of instabilities in GW2's order
// T2 has 1 instability, T3 has 2 instabilities, ...
type DayInstabilities<N extends Tier> = N extends `T2`
  ? [number]
  : N extends `T3`
    ? [number, number]
    : [number, number, number];

// zero-indexed list of instabilities for a whole *leap* year
type Days<N extends Tier> = DayInstabilities<N>[];

type LocalizedString = { de: string; en: string; es: string; fr: string };

export interface Instabilities {
  instabilities: { [x in FractalLevel<`T2`>]: Days<`T2`> } & {
    [x in FractalLevel<`T3`>]: Days<`T3`>
  } & { [x in FractalLevel<`T4`>]: Days<`T4`> },

  // list of instability details, indexed in `instabilities`
  instability_details: {
    evtc_id: number;
    icon_id: number;
    name: LocalizedString;
  }[],
}

export interface Dailies {
  dailies: [string, string, string][],
}

type RecommendedFractal = { scale: number; achievement_id: number };

export interface Recommended {
  recommended: [RecommendedFractal, RecommendedFractal, RecommendedFractal][],
}

export interface Fractals {
  scales: { scale: number, type: string, ar: number, daily_achievement_id: number }[],
  fractal_details: Record<string, { name: LocalizedString, scales: number[] }>
}
```

# Credits & Thanks to

- The fractal guild who initially discovered it.
- [Discretize [dT]](https://discretize.eu/) for putting the required info into the open.
- [itsmefox](https://github.com/itsmefox) for writing and providing Aleeva's API.
- [darthmaim](https://github.com/darthmaim) for maintenance and additional data.
- [greaka](https://github.com/greaka) for assisting with tooling and enduring my Rust skills.
