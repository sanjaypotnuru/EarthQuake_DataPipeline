# EarthQuake Azure Pipeline

Simple ETL pipeline that ingests earthquake events from the USGS API and stages/enriches them through Bronze → Silver → Gold Databricks notebooks. Orchestration is done in Azure Data Factory / Synapse.

## Files
- DataBricks_Notebook/Bronze Notebook.ipynb — ingest USGS GeoJSON, save raw JSON to Bronze.
- DataBricks_Notebook/Silver Notebook.ipynb — flatten / normalize and write Parquet to Silver.
- DataBricks_Notebook/Gold Notebook.ipynb — enrich with reverse geocoding, classify significance, write Parquet to Gold.
- Azure Synapse Sql Script Code.txt — example SQL to query Gold Parquet.

## Prerequisites
- ADLS Gen2 storage with containers: `bronze`, `silver`, `gold`.
- Databricks cluster with libraries: `requests`, `reverse_geocoder`.
- Cluster access to ADLS (service principal, managed identity, or mounted credentials).
- Azure Data Factory / Synapse pipeline configured to run notebooks in order (Bronze → Silver → Gold).

## Execution / Parameters
- Bronze notebook widgets: `start_date`, `end_date` (strings).
- Bronze returns JSON with ADLS paths which is passed to Silver as `bronze_params`.
- Silver returns output path passed to Gold as `silver_params`.
- Gold reads Silver parquet, enriches data, and writes to Gold parquet.

## Notes & Troubleshooting
- Confirm timezone/epoch handling when filtering by date. Notebooks convert UNIX millis to timestamp.
- Ensure `reverse_geocoder` is installed on all cluster workers.
- Use notebook logs to inspect returned JSON between activities when troubleshooting ADF/Synapse mappings.

## Quick Git steps (from repo root)
- Save the README as `README.md` at the repository root.
- Commit and push:
```bash
git add README.md
git commit -m "Add README for EarthQuake Azure Pipeline"
git push origin main
```

## License
- Add a
