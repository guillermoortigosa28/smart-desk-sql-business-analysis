# Data access and publication notes

The project owner has confirmed permission to publish the three course datasets. Copies are included in `data/`.

For data minimization, the published `Accounts.csv` excludes `CONTACT_EMAIL` and `ACCOUNT_EXECUTIVE`; the original file is not redistributed. The account, industry, country and region fields required for the SQL analysis remain. `Sales.csv` and `Forecasts.csv` are included as supplied.

Before publishing, independently verify the permission covers the complete dataset and that any remaining account or contact names are appropriate for public distribution. Do not upload credentials, private Snowflake screenshots, or unredacted versions.

## Reproducibility

Import the CSVs into Snowflake as `SALES`, `ACCOUNTS` and `FORECASTS`, then execute `sql/01_business_questions.sql` and `sql/02_exploratory_analysis.sql`. The revised SQL has not yet been tested in a live Snowflake environment.
