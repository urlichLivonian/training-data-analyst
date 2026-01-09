# Data Ingestion Pipeline Run

https://partner.skills.google/paths/84/course_templates/627/labs/593306

Before you begin, run `gcloud services enable cloudaicompanion.googleapis.com`

and `gcloud storage cp -r gs://spls/gsp290/dataflow-python-examples .`

`gcloud storage buckets create gs://BUCKET_NAME --location=REGION`

`gcloud storage cp gs://spls/gsp290/data_files/usa_names.csv gs://BUCKET_NAME/data_files/`

`gcloud storage cp gs://spls/gsp290/data_files/head_usa_names.csv gs://BUCKET_NAME/data_files/`


## Task 1. Review and run the data ingestion pipeline

Prompt for the following:

```text
You are an expert Data Engineer at Cymbal AI. A new team member is unfamiliar with this pipeline code. Explain the purpose and functionality of the data ingestion pipeline defined in the data_ingestion.py. Your explanation should include:

1. A high-level summary of what the script does.
2. A breakdown of the key components, such as the DataIngestion class and the run function.
3. An explanation of how the script uses the Apache Beam pipeline to read, process, and write data.
4. The role of command-line arguments and how they are used.
5. A description of the input data format and the output BigQuery table schema.

For the suggested improvements, don't update this file.
```

### Run the code in a container

```bash
cd ~
docker run -it -e PROJECT=qwiklabs-gcp-03-1ba697537590 -v $(pwd)/dataflow-python-examples:/dataflow python:3.8 /bin/bash
```

Once the container finishes pulling and starts executing in Cloud Shell, run the following to install apache-beam in that running container:

```bash
pip install apache-beam[gcp]==2.59.0
```

Next, in the running container in the Cloud Shell, change directories into where you linked the source code:

```bash
cd /dataflow
```

Run the following code to execute the data ingestion pipeline:

```bash
python dataflow_python_examples/data_ingestion.py \
  --project=qwiklabs-gcp-03-1ba697537590 \
  --region=europe-west1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://qwiklabs-gcp-03-1ba697537590/test \
  --temp_location gs://qwiklabs-gcp-03-1ba697537590/test \
  --input gs://qwiklabs-gcp-03-1ba697537590/data_files/head_usa_names.csv \
  --save_main_session
```

## Task 2. Review and run the data transformation pipeline

In this task, you review the data transformation pipeline to learn how it works. You then run the pipeline to process the Cloud Storage files and output the result to BigQuery.

The data transformation pipeline also ingests data from Cloud Storage into the BigQuery table using a TextIO source and a BigQueryIO destination, but with additional data transformations. Specifically, the pipeline:

- Ingests the files from Cloud Storage.
- Converts the lines read to dictionary objects.
- Transforms the data which contains the year to a format BigQuery understands as a date.
- Outputs the rows to BigQuery.

Prompt for the following:

```text
You are an expert Data Engineer at Cymbal AI. A new team member is unfamiliar with this pipeline code. Explain the purpose and functionality of the data transformation pipeline defined in the data_transformation.py. Your explanation should include:

1. A high-level summary of what the script does, noting its differences from a simple ingestion pipeline.
2. A breakdown of the key components, specifically the DataTransformation class and the run function.
3. A detailed explanation of how the script uses the Apache Beam pipeline to read from a file, transform the data, and write it to a BigQuery table.
4. Describe how the script handles the BigQuery schema by reading it from a JSON file.
5. Explain the data transformation logic within the parse_method, particularly how it converts the year to a DATE type.
6. The role of command-line arguments and how they are used.

For the suggested improvements, don't update this file.
```

Enter the following command in the Cloud Shell Terminal to run the data transformation pipeline:

```bash
python dataflow_python_examples/data_transformation.py \
  --project=qwiklabs-gcp-03-1ba697537590 \
  --region=europe-west1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://qwiklabs-gcp-03-1ba697537590/test \
  --temp_location gs://qwiklabs-gcp-03-1ba697537590/test \
  --input gs://qwiklabs-gcp-03-1ba697537590/data_files/head_usa_names.csv \
  --save_main_session
```

# Task 3. Review and run the data enrichment pipeline

You now build a data enrichment pipeline that accomplishes the following:

Ingest the files from Cloud Storage.
Filter out the header row in the files.
Convert the lines read to dictionary objects.
Output the rows to BigQuery.

Prompt for the following:

```text
In the data_enrichment.py file, update line 83 by replacing x.decode('utf8') with x.
```

Enter the following command in the Cloud Shell Terminal to run the data enrichment pipeline:

```bash
python dataflow_python_examples/data_enrichment.py \
  --project=qwiklabs-gcp-03-1ba697537590 \
  --region=europe-west1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://qwiklabs-gcp-03-1ba697537590/test \
  --temp_location gs://qwiklabs-gcp-03-1ba697537590/test \
  --input gs://qwiklabs-gcp-03-1ba697537590/data_files/head_usa_names.csv \
  --save_main_session
```

# Task 4

Now you build a Dataflow pipeline that reads data from two BigQuery data sources and then joins the data sources. Specifically, you:

- Ingest files from two BigQuery sources.
- Join the two data sources.
- Filter out the header row in the files.
- Convert the lines read to dictionary objects.
- Output the rows to BigQuery.

```bash
python dataflow_python_examples/data_lake_to_mart.py \
  --worker_disk_type="compute.googleapis.com/projects//zones//diskTypes/pd-ssd" \
  --max_num_workers=4 \
  --project=qwiklabs-gcp-03-1ba697537590 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://qwiklabs-gcp-03-1ba697537590/test \
  --temp_location gs://qwiklabs-gcp-03-1ba697537590/test \
  --save_main_session \
  --region=europe-west1
```
