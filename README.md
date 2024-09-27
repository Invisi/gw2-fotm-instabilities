# GW2 Fractal Instabilties for T4

This work is licensed under [![CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/).

## How to use

There's basically two options at the moment:

- Use [Aleeva](https://aleeva.io)'s API. At the moment the API documentation is not finished but you can just contact itsmefox on the Aleeva Discord server.
- Integrate [data.json](data.json) into your own bot. The `instabilities` object contains the fractal level as key and the corresponding instabilities as values
  - e.g. `data["instabilities"]["76"][0]` gives the instabilities for level 76 on `????-01-01`.

### But I don't want to write my own bot!

- [Aleeva](https://aleeva.io)
- [GW2Bot](https://gw2bot.info/) ([repository](https://github.com/Maselkov/GW2Bot))
- [Discretize Discord Instability Bot](https://github.com/discretize/discretize-discord-bot-instabilities)

### Where else can I find the data?

- [@a727891](https://github.com/a727891)'s [Clears Tracker](https://github.com/a727891/BlishHud-Raid-Clears) [Blish Hud module](https://blishhud.com/modules/?module=Soeed.RaidClears)
- [@darthmaim](https://github.com/darthmaim)'s [GW2Treasures.com](https://gw2treasures.com/about) ([repository](https://github.com/GW2Treasures/gw2treasures.com))

## The format of `data.json`

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
type Instabilities = Array<number>;

// zero-indexed list of instabilities for a whole *leap* year
type Days = Array<Instabilities>;

export interface Data {
  instabilities: { [x in FractalLevel]: Days };
  // list of instability names, indexed in `instabilities`
  instability_names: string[];
}
```

**🛑 Keep in mind that <span style="color:red">the index of `Days` does include the leap day</span> and you will have account for that. 🛑**

# Credits & Thanks to

- The fractal guild who initially discovered it.
- [Discretize [dT]](https://discretize.eu/) for putting the required info into the open.
- [itsmefox](https://github.com/itsmefox) for writing and providing Aleeva's API.
- Invisi (this guy) for being a data hoarder and automating the collection of this data.
