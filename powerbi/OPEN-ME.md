# Opening the Power BI report

`SchoolCampusBuildings.pbip` is a Power BI Project: the report (`SchoolCampusBuildings.Report`, PBIR format) and the semantic model (`SchoolCampusBuildings.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine, wherever you cloned or unzipped it.

## Open it

1. In Windows, open File Explorer and go to the `powerbi` folder, wherever you cloned or unzipped the repo.
2. Double-click `SchoolCampusBuildings.pbip`, or in Desktop: **File, Open report, Browse reports**, file type *Power BI project files (\*.pbip)*.
3. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-school-campus/main/data/`). If Desktop asks how to connect, choose **Anonymous** and **Connect**.
   - To read the files on disk instead: **Home, Transform data, Edit parameters**, set `DataFolder` to the full path of the repo's `data` folder **ending with a backslash**, then **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** for this file (all sources are public open data) or set them to Public.
4. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh** (there are no slicers; every card is fixed to the year in its title):

| Page | Card | Should read |
|---|---|---|
| Building by building | Buildings | 13 |
| Building by building | Footprint m² | 9,484 |
| Building by building | Floor m² (estimated) | 23,624 |
| Site against buildings | Grounds m² per student, 2025 | 12.2 |
| Site against buildings | Footprint m² per student, 2025 | 4.8 |
| Site against buildings | Floor space m² per student, 2025 (estimated) | 11.9 |
| Site against buildings | Students, February 2025 (FTE) | 1,983 |

These are the README's figures. With nothing clicked, the **Over time** line runs from 1,210 students in 2007 to 1,990 in 2026, and the footprint columns stop at the 2023 snapshot.

The sample pages (not about any real building) read: Assets 226, High or extreme 40, Past expected life 49; Open 36, Overdue 17, Raised in 12 months 802; Planned maintenance on time 86.9%, Safety measures overdue 7, Capital works backlog 3,922,000.

## Site level and building level

`Buildings` has one row per building (the key the shapes carry). `Site` is one row: the two council parcels as one shape, with the buildings rolled up in Plotlay. `Enrolments` is the school's, one row per year. The per-student measures divide site totals by the school's enrolment; there is no per-building student figure, because enrolments are never split across buildings.

## The sample data pages

Three pages read the tables in `data/sample/`: **Sample: risk** (the risk matrix, the assets by risk, each building's assets at high or extreme risk), **Sample: work orders** (open by priority and building, raised each month, reactive against planned) and **Sample: compliance** (planned maintenance on time, essential safety measures by status, the capital works backlog). The tables are `SampleSummary`, `SampleAssets`, `SampleRisk`, `SampleWorkOrders`, `SamplePM` and `SampleESM`; their measures are in the Sample data folder. They are sample data, not about University High School or any real building, and each page carries a Sample data tag. No other page or measure reads them.

## The maps

- **Shape Map** (Building by building, and Sample: risk). The custom map, `data/geometry/campus_buildings.topojson`, is saved inside the report (`SchoolCampusBuildings.Report/StaticResources/RegisteredResources/campus_buildings.topojson`, a copy that keeps only the building key), so the thirteen buildings draw on open: by era on the first page, by sample assets at high or extreme risk on the risk page.
- If a map shows the United States instead (an older Desktop can ignore a saved map): select it, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\campus_buildings.topojson`. Under **View map keys** the keys B01 to B13 should match. Projection: Mercator. If the Shape Map is not in the Visualizations pane, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- If a Shape Map comes up empty after opening, drag the fields back: Location `Buildings[key]`, Legend `Buildings[era]` (first page) or Color saturation `[Sample high or extreme]` (risk page).
- Optional, **Azure Maps**: add a reference layer from `..\data\geometry\campus_buildings.geojson`, join property `key`, map style **Grayscale (light)** or **Blank**. Do not publish satellite basemap screenshots.

## Save a .pbix too

After the refresh: **File, Save as**, type **Power BI file (\*.pbix)**, save as `powerbi\SchoolCampusBuildings.pbix`. It includes the data.

## Publish (only if you choose to)

1. **Home, Publish**, sign in, pick a workspace.
2. In the service, the semantic model's **Settings, Data source credentials**: the GitHub address uses **Anonymous**. Scheduled refresh then works with no gateway.
3. **Publish to web** makes a public embed link; anyone with the link can see the report. Only if you intend it to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), Plotlay theme on. Snipping Tool (Win+Shift+S) for stills, Win+Alt+R or Clipchamp for clips. Save to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Building by building | The Shape Map coloured by era, the cards and the table | still |
| 2 | Pick a building | Click B01 in the table: the map and cards follow, its floor share 75.9% | 3 s clip |
| 3 | Site against buildings | The four cards (grounds, footprint, floor space per student; students) and the bar chart | still |
| 4 | Over time | Hover 2020 and 2023 on the footprint chart: the six single-storey buildings arrive | 4 s clip |
| 5 | Sample: risk | The risk matrix and the assets by risk. Keep the Sample data tag and the footer in shot | still |
| 5a | Sample: work orders | Click P3 Routine: the buildings and months follow | 3 s clip |
| 5b | Sample: compliance | Planned maintenance on time by month and the safety measures by building | still |
| 6 | Model view | The tables, Buildings in the middle, the sample tables related to it by building key | still |
| 7 | Power Query | **Transform data**: the `DataFolder` parameter and the steps of one sample table | still |

Keep the footer with the attribution visible in every shot.

## What's in the folder

- `SchoolCampusBuildings.pbip`: open this.
- `SchoolCampusBuildings.SemanticModel/definition/`: `tables/*.tmdl` (columns, measures, Power Query), `relationships.tmdl`, `expressions.tmdl` (the `DataFolder` parameter and a small `GetFile` function).
- `SchoolCampusBuildings.Report/definition/`: one folder per page and visual (PBIR), `report.json` (the Plotlay theme registered).
- Pages: Building by building, Site against buildings, Over time, Sample: risk, Sample: work orders, Sample: compliance.
