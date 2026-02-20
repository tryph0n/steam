# Steam Market Analysis

Big data analysis of the Steam gaming platform (55,690 games) to identify market trends, success factors, and strategic recommendations for Ubisoft. Built with PySpark on Databricks.

### Link to databricks notebooks

```
 Full html outputs are in `nb_ouput` folder: to be opened with any web browser.
```

- [ETL](https://dbc-7718d0bf-eba6.cloud.databricks.com/editor/notebooks/479238405993163?o=2944035511361577)

- [Macro_Analysis](https://dbc-7718d0bf-eba6.cloud.databricks.com/editor/notebooks/479238405993162?o=2944035511361577)

- [Genre_Analysis](https://dbc-7718d0bf-eba6.cloud.databricks.com/editor/notebooks/4285845165851947?o=2944035511361577)

- [Platform_Analysis](https://dbc-7718d0bf-eba6.cloud.databricks.com/editor/notebooks/479238405993161?o=2944035511361577)

- [Popularity_Factors](https://dbc-7718d0bf-eba6.cloud.databricks.com/editor/notebooks/4285845165851948?o=2944035511361577)

## Data Source

`s3://full-stack-bigdata-datasets/Big_Data/Project_Steam/steam_game_output.json` -- nested JSON snapshot from SteamSpy, collected circa late 2022.

## Notebooks

Run in order. `00_ETL` produces the `steam_clean` table consumed by all subsequent analyses.

| #   | Notebook                | Focus                                                                                                                                                                                                              |
| --- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0   | `00_ETL`                | Load raw JSON from S3, flatten, clean types, engineer features (`avg_owners`, `rating_ratio`, `estimated_revenue`, ...), persist as `steam_clean`                                                                  |
| 1   | `01_Macro_Analysis`     | Publishers, best-rated games (Bayesian score, m=100000), releases over time (YoY Covid via self-join), pricing, language coverage, age ratings                                                                     |
| 2   | `02_Genre_Analysis`     | Genre popularity, average ratings, publisher-genre preferences, estimated revenue by genre                                                                                                                         |
| 3   | `03_Platform_Analysis`  | Windows/Mac/Linux availability and historical trends, genre-platform deviations, category breakdown (multiplayer, VR)                                                                                              |
| 4   | `04_Popularity_Factors` | Combined factor analysis: price, genre, platform, language tiers (Extensive = 11+), F2P vs paid, temporal trends, publisher effect, top-game segmentation (Top 100/1000 via threshold-based cuts), category impact |

Each notebook ends with a conclusion section summarizing findings.

## Technical notes

- Pure PySpark -- no Window functions, no UDFs
- `pos_neg_ratio` kept at full precision (not rounded)
- Categories sorted by `avg_owners` (not game count)
- All temporal analysis uses ~2022 as reference year (dataset boundary)

## How to run

1. Import all notebooks into a Databricks workspace with a PySpark 3.x cluster
2. Ensure the cluster has read access to the S3 source bucket
3. Run notebooks in order (00 through 04)
