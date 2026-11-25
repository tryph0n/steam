# Steam Market Analysis

Big data analysis of the Steam gaming platform (55,690 games) to identify market trends, success factors, and strategic recommendations for a game publisher. Built with PySpark on Databricks.

Fictional business case (Ubisoft as client) from a data training course. No affiliation with Ubisoft or Valve.

## Data Source

SteamSpy snapshot (nested JSON, collected around late 2022), hosted by the Jedha data science training on a public S3 bucket: `s3://full-stack-bigdata-datasets/Big_Data/Project_Steam/steam_game_output.json`. Not included in this repository. No licence stated by the source; the data is not covered by the code licence.

## Notebooks

Run in order. `00_ETL` produces the `steam_clean` table consumed by all subsequent analyses. Each notebook has a full export with outputs and charts in `nb_output/`.

| # | Notebook | Full output with charts | Focus |
| --- | --- | --- | --- |
| 0 | [00_ETL.ipynb](00_ETL.ipynb) | [00_ETL.html](nb_output/00_ETL.html) | Load raw JSON from S3, flatten, clean types, engineer features (`avg_owners`, `rating_ratio`, `estimated_revenue`, ...), persist as `steam_clean` |
| 1 | [01_Macro_Analysis.ipynb](01_Macro_Analysis.ipynb) | [01_Macro_Analysis.html](nb_output/01_Macro_Analysis.html) | Publishers, best-rated games (Bayesian score, m=100000), releases over time (YoY Covid via self-join), pricing, language coverage, age ratings |
| 2 | [02_Genre_Analysis.ipynb](02_Genre_Analysis.ipynb) | [02_Genre_Analysis.html](nb_output/02_Genre_Analysis.html) | Genre popularity, average ratings, publisher-genre preferences, estimated revenue by genre |
| 3 | [03_Platform_Analysis.ipynb](03_Platform_Analysis.ipynb) | [03_Platform_Analysis.html](nb_output/03_Platform_Analysis.html) | Windows/Mac/Linux availability and historical trends, genre-platform deviations, category breakdown (multiplayer, VR) |
| 4 | [04_Popularity_Factors.ipynb](04_Popularity_Factors.ipynb) | [04_Popularity_Factors.html](nb_output/04_Popularity_Factors.html) | Combined factor analysis: price, genre, platform, language tiers (Extensive = 11+), F2P vs paid, temporal trends, publisher effect, top-game segmentation (Top 100/1000 via threshold-based cuts), category impact |

Each analysis notebook (01 to 04) ends with a conclusion section summarizing findings. Notebook commentary is in French.

### Viewing the outputs

- The .ipynb files show code and tables. The 30 charts were built with the Databricks visualization tool and appear only in the HTML exports; the notebooks show a placeholder.
- GitHub shows the source of .html files, not the page. Download the file and open it in a web browser.
- The HTML exports load their scripts and styles from the Databricks CDN, so they need an internet connection to render.

## Technical notes

- Pure PySpark -- no Window functions, no UDFs
- `pos_neg_ratio` kept at full precision (not rounded)
- Categories sorted by `avg_owners` (not game count)
- All temporal analysis uses ~2022 as reference year (dataset boundary)

## How to run

The notebooks run on Databricks only. They use the Databricks `spark` session, `display()` and charts configured in the Databricks UI, so they do not run locally as-is.

1. Import all notebooks into a Databricks workspace. Use compute with Spark 3.4 or later (the ETL uses the SQL function `get`; the recorded session is Spark Connect), for example serverless compute or Databricks Runtime 13.3 LTS or later.
2. Ensure the compute has read access to the S3 source bucket
3. Run notebooks in order (00 through 04)

## License

Code and notebooks: MIT, see [LICENSE](LICENSE). The data is not included and not covered by this licence.
