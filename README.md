# GW2 Fractal Instabilties for T4

This work is licensed under [![CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/).

## How to use

There's basically two options at the moment:

- Use [Aleeva](https://aleeva.io)'s API. Documentation available [here](https://api.aleeva.io/api-doc.html).
- Integrate the data in this repository directly in your project. This repository contains 4 JSON files:
  - **`instabilities.json`**: Contains the instabilities for a given T4 fractal on a given day.  
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
// looks funny but that's how you can get 76-100 as strings
type LowerLevels = `6` | `7` | `8` | `9`;
type UpperLevels = `0` | `1` | `2` | `3` | `4` | `5` | LowerLevels;
type FractalLevel =
  | `7${LowerLevels}`
  | `8${UpperLevels}`
  | `9${UpperLevels}`
  | `100`;

// list of instabilities in GW2's order
type Instabilities = [number, number, number];

// zero-indexed list of instabilities for a whole *leap* year
type Days = Instabilities[];

type LocalizedString = { de: string, en: string, es: string, fr: string };

type RecommendedFractal = { scale: number, achievement_id: number }

export interface Instabilities {
  instabilities: { [x in FractalLevel]: Days },

  // list of instability names, indexed in `instabilities`
  instability_details: { icon_id: number, name: LocalizedString }[],
}

export interface Dailies {
  dailies: [string, string, string][],
}

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
- Invisi (this guy) for being a data hoarder and automating the collection of this data.
