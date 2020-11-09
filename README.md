# GW2 Fractal Instabilties for T4
This work is licensed under [![CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/).

## How to use this
There's basically two options at the moment:
- Use [Aleeva](https://aleeva.io)'s API. At the moment the API documentation is not finished but you can just contact itsmefox#0001 on Discord or join the Aleeva server.
- Integrate [data.json](data.json) into your own bot. The `instabilities` object contains the fractal level as key and the corresponding instabilities as values
  - e.g. `data["instabilities"]["76"][0]` gives the instabilities for level 76 on `????-01-01`.

## The format of `data.json`
Here's a commented TypeScript interface.
```ts
enum FractalLevel {
    "76",
    // ...,
    "100"
}

type Instabilities = Array<number>; // List of instabilities in GW2's order
type Days = Array<Instabilities>; // Zero-indexed list of instabilities for a whole year

export interface Data {
    instabilities: { [x in FractalLevel]: Days };
    instability_names: string[]; // List of instability names, indexed in `instabilities`
}
```
**🛑 Keep in mind that <span style="color:red">the index of `Days` does include the leap day</span> and you will have account for that. 🛑**

# Credits & Thanks to
- The fractal guild who initially discovered it.
- [Discretize [dT]](https://discretize.eu/) for putting the required info into the open.
- [itsmefox](https://github.com/itsmefox) for writing and providing Aleeva's API.
- Invisi (this guy) for being a data hoarder and automating the collection of this data.
