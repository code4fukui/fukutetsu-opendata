# fukutetsu-opendata

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

Open data repository for the Fukui Railway (Fukutetsu) timetable in Japan. This project scrapes, processes, and provides the schedule as structured CSV data.

## Demo

A live departure countdown viewer built with this data:

**[https://code4fukui.github.io/fukutetsu-opendata/](https://code4fukui.github.io/fukutetsu-opendata/)**

The web app allows you to select a departure and arrival station to see a real-time countdown for the next available trains in both directions.

## Features

-   **Scraper**: A Deno script (`scrapeTimetable.js`) to fetch the latest timetable from the official Fukui Railway website.
-   **Data Processor**: A script (`makeTimetable.js`) that cleans the raw scraped data and combines it into a single, easy-to-use CSV file.
-   **Structured Data**: Provides timetable data in CSV format, including raw up/down files and a combined, processed file.
-   **JavaScript Helper**: A utility class (`Timetable.js`) to easily query train schedules, find routes, and get upcoming departures from the processed data.
-   **Live Demo**: A complete web application (`index.html`) demonstrating a practical use of the data.

## Data Files

The data is scraped from the [Fukui Railway Timetable Page](https://fukutetsu.jp/train/timetable_all.php).

-   `fukutetsu-timetable-kudari.csv`: Raw scraped data for the "down" line (kudari).
-   `fukutetsu-timetable-nobori.csv`: Raw scraped data for the "up" line (nobori).
-   `fukutetsu-timetable.csv`: The combined and processed data file, ideal for programmatic use. This is the file used by the demo app and `Timetable.js`.

## Usage

You can use the `Timetable.js` class in any Deno or browser-based JavaScript project to work with the timetable data.

First, load the processed CSV data and instantiate the class:

```javascript
import { Timetable } from "./Timetable.js";
import { CSV } from "https://js.sabae.cc/CSV.js";

// Load the processed timetable data
const data = await CSV.fetchJSON("fukutetsu-timetable.csv");
const timetable = new Timetable(data);
```

Then, you can query for train schedules:

```javascript
// Get all available trains between two stations
const trains = timetable.getTrains("西鯖江", "福井駅");
console.log(trains);

// Get the next 3 upcoming trains from a station at the current time
const nextTrains = timetable.getNextTrains("福井駅", "西鯖江", 3);
console.log(nextTrains);

// Get a list of all station names
const stations = timetable.getStations();
console.log(stations);
```

## License

MIT License — see [LICENSE](LICENSE).