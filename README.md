#Setup Jmeter for CockroachDB

Files: \
`Cockroachdb.jmx` -- Jmeter test plan
`jmeter-plugins-manager-1.4.jar` -- Jmeter plugin

Sample DataSets: \
`fandango_score_comparison_data.csv` -- fandango_score_comparison_data table dataset
`fandango_scrape_data.csv`  -- fandango_scrape_data table dataset

JDBC Driver required: \
`postgresql-42.2.2.jar` - Postgress JDBC driver which is compatiable with CRDB latest version

Test dataset that is used as variable data: \
`loadtest_numbers.csv: `

Importing and creating the tables in CRDB required for the test case:

```
cockroach nodelocal upload <download_location>/fandango_scrape_data.csv fandango_scrape_data.csv --host=localhost:26257 --insecure 

cockroach nodelocal upload <download_location>/fandango_score_comparison_data.csv  fandango_score_comparison_data.csv --host=localhost:26257 --insecure

IMPORT TABLE FANDANGO.FANDANGO_SCRAPE  (
        FILM  STRING ,
        STARS DECIMAL,
        RATING  DECIMAL,
      VOTES INT,
   CONSTRAINT "PRIMARY" PRIMARY KEY (FILM ASC)
)
CSV DATA ('nodelocal://1/fandango_scrape_data.csv')
;

IMPORT TABLE FANDANGO.FANDANGO_SCORE_COMPARISON (
FILM STRING ,
ROTTENTOMATOES INT,
ROTTENTOMATOES_USER INT,
METACRITIC DECIMAL,
METACRITIC_USER DECIMAL,
IMDB DECIMAL,
FANDANGO_STARS DECIMAL,
FANDANGO_RATINGVALUE DECIMAL,
RT_NORM DECIMAL,
RT_USER_NORM DECIMAL,
METACRITIC_NORM DECIMAL,
METACRITIC_USER_NOM DECIMAL,
IMDB_NORM DECIMAL,
RT_NORM_ROUND DECIMAL,
RT_USER_NORM_ROUND  DECIMAL,
METACRITIC_NORM_ROUND  DECIMAL,
METACRITIC_USER_NORM_ROUND DECIMAL,
IMDB_NORM_ROUND DECIMAL,
METACRITIC_USER_VOTE_COUNT DECIMAL,
IMDB_USER_VOTE_COUNT DECIMAL,
FANDANGO_VOTES DECIMAL,
FANDANGO_DIFFERENCE DECIMAL,
ID INT,
CONSTRAINT "PRIMARY" PRIMARY KEY (ID)
)
CSV DATA ('nodelocal://1/fandango_score_comparison_data.csv');
```
