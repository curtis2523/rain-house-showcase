# Rain House

[![Rain House Today home screens in the native iOS app and the desktop Web app](assets/product/rain-house.webp)](assets/product/rain-house.webp?raw=1)

I designed and built Rain House for iOS and the Web. [Open Rain House on the Web](https://rainhouse.top)

## iOS product design

<p align="center">
  <a href="assets/product/daily-lyrics.webp?raw=1"><img src="assets/product/daily-lyrics.webp" width="370" alt="Daily Lyrics with the Today view and an exported lyric card"></a>&emsp;<a href="assets/product/song-rankings.webp?raw=1"><img src="assets/product/song-rankings.webp" width="370" alt="Song Rankings with a pairwise matchup and the exported Top 10"></a>
</p>
<p align="center">
  <a href="assets/product/concert-diary.webp?raw=1"><img src="assets/product/concert-diary.webp" width="370" alt="Concert Diary with the tour archive and the setlist for an individual show"></a>&emsp;<a href="assets/product/widgets.webp?raw=1"><img src="assets/product/widgets.webp" width="370" alt="WidgetKit views for a daily lyric and the countdown to the next show"></a>
</p>

Select a board to view it at full resolution.

## Implementation

- **iOS** — Swift and SwiftUI for the app, with a shared Swift package for the catalog, local persistence, and Elo ratings. WidgetKit reads daily lyrics and concert countdowns from shared App Group data.
- **Web** — TypeScript, React, and Taro for a responsive H5 app with browser storage and file adapters. I deployed the build on Cloudflare Pages and migrated the production site to EdgeOne Pages. [Desktop dashboard](assets/web-desktop-dashboard.webp)
- **Content Studio** — A local Python tool for searching and editing releases, songs, concerts, and ordered setlists. It validates catalog references and keeps a journal for restoring prior revisions. [Setlist editing demo](assets/catalog-workflow.mp4)

## Database Design & Data Management

The clients start with a bundled JSON catalog. On iOS, the app writes personal state as JSON and shares a snapshot with the widgets through an App Group. The Web app stores a versioned personal document in IndexedDB. JSON backup export and restore support manual transfer of compatible iOS data to the Web.

<img src="assets/product/data-architecture.webp" width="420" alt="Rain House data flow from Python Content Studio through the JSON catalog to SwiftUI and Taro. The iOS App Group shares data with WidgetKit and the Web app uses IndexedDB.">

<details>
<summary>Catalog model, storage options, and backup</summary>

- **Music catalog** — An album contains songs. Lyric records refer to a song by `catalogSongID`, and excerpts link by `songID` with a `dailyEligible` flag for selection.
- **Concert catalog** — A setlist links to a concert by `concertID` and contains ordered sections. Each entry can reference a song or use a manual title. In a fictional example, `demo-show-1` links to a setlist whose Opening section references `demo-track-1`, “Paper Lanterns.”
- **Personal state** — Favorites reference excerpt IDs, ratings are keyed by song ID, and concert memories are keyed by event ID. Supported iOS builds can store a JSON document in iCloud Drive and mirror a snapshot to the App Group.
- **Publishing and accounts** — The catalog publisher supports versioned snapshots on Tencent Cloud COS with hash verification and rollback. The live Web deployment serves its bundled catalog. I used CloudBase Auth and PostgreSQL-backed Storage for avatars in a separate account preview.

[Web backup controls](assets/web-mobile-backup.webp) · [Studio setlist editor](assets/studio-setlist-editor.webp) · [Studio history and restore](assets/studio-history.webp)

</details>

The Rain House app and Content Studio source code are private.
