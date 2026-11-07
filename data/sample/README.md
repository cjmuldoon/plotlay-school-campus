# Sample data

`sample_facilities.csv` is sample data: the figures are invented for illustration and are not about University High
School or any real building. It shows where a facilities team's own register goes once the buildings are mapped: condition, maintenance backlog,
electricity and water by building meter, teaching spaces, capacity against timetable use, the asbestos register and
essential safety measures.

How it was made, so anyone can make it again: Python's `random.Random(20261107)`, the buildings in key order, each
column drawn in a plausible range and scaled by the building's estimated floor area (`floor_m2_est` in
`data/buildings/buildings.csv`). Every column starts with `sample_`, every row says `Sample data, invented`, and the
file opens with a comment line (read it with `pandas.read_csv(..., comment='#')`, or skip the first row in Power Query).

| Column | Drawn as |
|---|---|
| sample_condition_1to5, sample_condition | 2 plus a draw of 0 to 3; one lower for a building on the 2014 outlines, one higher for one first shown after 2014 |
| sample_backlog_aud | footprint m² x a draw of 40 to 140 x (6 minus condition), to the nearest 1,000 |
| sample_electricity_kwh_yr | floor m² x a draw of 35 to 95, to the nearest 100 (under 50 m² of floor: a draw of 300 to 1,500) |
| sample_water_kl_yr | floor m² x a draw of 0.15 to 0.45, to the nearest 10 (under 50 m²: 0) |
| sample_teaching_spaces | floor m² / 250, rounded (single-storey buildings of 150 to 250 m²: 2; under 50 m²: 0) |
| sample_capacity_seats | teaching spaces x 25 |
| sample_timetable_use_pct | a draw of 55 to 96 (none without teaching spaces) |
| sample_asbestos_register | on the map by 2014: Register current, or Register review due one time in three; later: Not expected (built after 2003) |
| sample_esm_status | All items current, or 1 item open one time in four |

The sample is never mixed into the real figures: no chart, median or total in this repository uses it except the
sample's own section, which is labelled Sample data.
