# fukutetsu-opendata

福井鉄道（福鉄）の時刻表オープンデータリポジトリです。本プロジェクトでは、時刻表をスクレイピングおよび処理し、構造化されたCSVデータとして提供します。

## デモ

このデータを利用して構築された、発車カウントダウンビューアー:

**[https://code4fukui.github.io/fukutetsu-opendata/](https://code4fukui.github.io/fukutetsu-opendata/)**

このウェブアプリでは、出発駅と到着駅を選択することで、双方向の次発列車のリアルタイムカウントダウンを確認できます。

## 機能

- **スクレイパー**: 福井鉄道の公式サイトから最新の時刻表を取得するDenoスクリプト（`scrapeTimetable.js`）。
- **データプロセッサ**: スクレイピングした生データを整形し、単一の使いやすいCSVファイルに結合するスクリプト（`makeTimetable.js`）。
- **構造化データ**: 上下線の生データファイルや、結合・処理済みのファイルを含む、CSV形式の時刻表データを提供します。
- **JavaScriptヘルパー**: 処理済みデータから列車の時刻表の検索、経路の確認、次発列車の取得を簡単に行えるユーティリティクラス（`Timetable.js`）。
- **ライブデモ**: データの実用例を示す完全なウェブアプリケーション（`index.html`）。

## データファイル

データは[福井鉄道 電車全列車時刻表](https://fukutetsu.jp/train/timetable_all.php)からスクレイピングしています。

-   `fukutetsu-timetable-kudari.csv`: 下り線（kudari）のスクレイピングされた生データ。
-   `fukutetsu-timetable-nobori.csv`: 上り線（nobori）のスクレイピングされた生データ。
-   `fukutetsu-timetable.csv`: 結合および処理済みのデータファイルで、プログラムでの利用に最適です。デモアプリや `Timetable.js` で使用されているファイルです。

## 使い方

DenoやブラウザベースのJavaScriptプロジェクトで `Timetable.js` クラスを使用し、時刻表データを扱うことができます。

まず、処理済みのCSVデータを読み込み、クラスをインスタンス化します。

```javascript
import { Timetable } from "./Timetable.js";
import { CSV } from "https://js.sabae.cc/CSV.js";

// 処理済みの時刻表データを読み込む
const data = await CSV.fetchJSON("fukutetsu-timetable.csv");
const timetable = new Timetable(data);
```

その後、列車の時刻表を検索できます。

```javascript
// 2つの駅間のすべての利用可能な列車を取得
const trains = timetable.getTrains("西鯖江", "福井駅");
console.log(trains);

// 現在時刻を基準に、ある駅から出発する次の3本の列車を取得
const nextTrains = timetable.getNextTrains("福井駅", "西鯖江", 3);
console.log(nextTrains);

// すべての駅名のリストを取得
const stations = timetable.getStations();
console.log(stations);
```

## ライセンス

MIT License — 詳細は [LICENSE](LICENSE) を参照してください。
