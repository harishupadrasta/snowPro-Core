# Domain 3: Data Loading, Unloading, and Connectivity - Quiz Questions

![Domain 3](https://img.shields.io/badge/Domain-3-blue)
![Questions](https://img.shields.io/badge/Questions-55-green)
![Weight](https://img.shields.io/badge/Exam_Weight-18%25-orange)
![Difficulty](https://img.shields.io/badge/Difficulty-Mixed-yellow)

---

## Stages and File Management

### Question 1
What command is used to upload files from a local file system to an internal stage in Snowflake?

- A) UPLOAD
- B) COPY INTO
- C) PUT
- D) INSERT INTO

**Answer: C)**

**Explanation:** The PUT command is used to upload (stage) files from a local file system to a Snowflake internal stage (named, table, or user stage). PUT is only available through SnowSQL or the JDBC/ODBC drivers, not through the Snowflake web UI.

---

### Question 2
Which of the following is NOT a type of internal stage in Snowflake?

- A) User stage
- B) Table stage
- C) Named stage
- D) Schema stage

**Answer: D)**

**Explanation:** Snowflake supports three types of internal stages: User stages (@~), Table stages (@%table_name), and Named stages (@stage_name). There is no "Schema stage" type in Snowflake.

---

### Question 3
What is the default prefix for referencing a user stage?

- A) @%
- B) @~
- C) @$
- D) @user/

**Answer: B)**

**Explanation:** The user stage is referenced with @~ (at-tilde). Each user has their own stage automatically. Table stages use @%table_name, and named stages use @stage_name.

---

### Question 4
A company stores its data files in an Amazon S3 bucket. Which type of Snowflake stage should they create to reference this location?

- A) Internal named stage
- B) External stage
- C) Table stage
- D) User stage

**Answer: B)**

**Explanation:** An external stage references data files stored in a location outside of Snowflake, such as Amazon S3, Google Cloud Storage, or Microsoft Azure Blob Storage. You create an external stage with CREATE STAGE specifying the URL and credentials.

---

### Question 5
Which command downloads files from a Snowflake internal stage to the local file system?

- A) DOWNLOAD
- B) COPY FROM
- C) GET
- D) EXPORT

**Answer: C)**

**Explanation:** The GET command downloads data files from a Snowflake internal stage to the local file system. Like PUT, GET is only available through SnowSQL or the JDBC/ODBC drivers.

---

### Question 6
A data engineer needs to list files in a stage. Which command should they use?

- A) SHOW FILES IN @my_stage
- B) LIST @my_stage
- C) DESCRIBE STAGE @my_stage
- D) SELECT * FROM @my_stage

**Answer: B)**

**Explanation:** The LIST (or LS) command lists the files in a stage. DESCRIBE STAGE shows the stage properties but not the files. SELECT from a stage would attempt to query the data content, not list files.

---

### Question 7
What happens by default when you PUT a file that already exists in the internal stage?

- A) The file is overwritten
- B) The upload fails with an error
- C) A duplicate is created with a suffix
- D) The file is skipped (not uploaded)

**Answer: A)**

**Explanation:** By default, the PUT command sets OVERWRITE=FALSE, which means the file is NOT uploaded if it already exists. However, if OVERWRITE=TRUE is set, the file is overwritten. Note: The actual default is OVERWRITE=FALSE, meaning files are skipped if they already exist. Let me correct: By default (OVERWRITE=FALSE), if the file already exists with the same name, the PUT command skips the upload. The correct answer should reflect that the default is to skip. Actually, the default behavior is that PUT will upload and overwrite the file. Let me verify: The PUT default for OVERWRITE is FALSE — the file is NOT uploaded if it already exists. Correct answer is D.

**Correction - Answer: D)**

**Explanation:** By default, PUT has OVERWRITE=FALSE. If a file with the same name already exists in the stage, the upload is skipped. Set OVERWRITE=TRUE to replace existing files.

---

### Question 8
Which of the following is TRUE about table stages?

- A) They can be altered or dropped independently
- B) They support file format options in the stage definition
- C) Each table has a stage automatically allocated to it
- D) They can be shared with other tables

**Answer: C)**

**Explanation:** Every table in Snowflake automatically has a table stage associated with it (referenced as @%table_name). Table stages cannot be altered, dropped, or shared — they are tied to the table's lifecycle. They also do not support setting file format options in the stage definition.

---

### Question 9
Your organization requires that credentials for accessing cloud storage are managed centrally and not embedded in stage definitions. What Snowflake object should you use?

- A) Security integration
- B) Storage integration
- C) Network policy
- D) Resource monitor

**Answer: B)**

**Explanation:** A storage integration is a Snowflake object that stores a generated identity and access management (IAM) entity for external cloud storage, along with an optional set of allowed or blocked storage locations. This avoids embedding credentials directly in stage definitions.

---

### Question 10
What is the maximum file size recommended for data loading in Snowflake for optimal performance?

- A) 10-50 MB
- B) 100-250 MB compressed
- C) 1-5 GB
- D) No limit recommended

**Answer: B)**

**Explanation:** Snowflake recommends files be 100-250 MB in compressed size for optimal parallel loading. Files that are too small create overhead; files too large reduce parallelism because they cannot be split across warehouse nodes (except for CSV which can be split).

---

## COPY INTO Command Options

### Question 11
What is the default value of the ON_ERROR option in the COPY INTO command?

- A) CONTINUE
- B) ABORT_STATEMENT
- C) SKIP_FILE
- D) SKIP_FILE_5

**Answer: B)**

**Explanation:** The default value of ON_ERROR for COPY INTO <table> is ABORT_STATEMENT, which aborts the entire load operation if any error is encountered in a data file. Other options include CONTINUE, SKIP_FILE, and SKIP_FILE_<num>|<num>%.

---

### Question 12
A data engineer wants to load data and skip any file that contains more than 10 errors. Which ON_ERROR setting should they use?

- A) ON_ERROR = SKIP_FILE
- B) ON_ERROR = SKIP_FILE_10
- C) ON_ERROR = CONTINUE
- D) ON_ERROR = ABORT_STATEMENT_10

**Answer: B)**

**Explanation:** ON_ERROR = SKIP_FILE_<num> skips a file when the number of errors in the file equals or exceeds the specified number. SKIP_FILE_10 will skip any file with 10 or more errors. SKIP_FILE skips files with any errors. CONTINUE loads all valid rows and skips individual error rows.

---

### Question 13
What does VALIDATION_MODE = 'RETURN_ERRORS' do?

- A) Loads data and returns errors in the result
- B) Validates all data without loading and returns all errors
- C) Validates the first 1000 rows without loading
- D) Loads data but logs errors to an error table

**Answer: B)**

**Explanation:** VALIDATION_MODE = 'RETURN_ERRORS' validates the entire data file without loading any data and returns all errors found. This is useful for checking data quality before performing the actual load. Note that VALIDATION_MODE and COPY INTO cannot load data simultaneously — it is a dry-run validation only.

---

### Question 14
Which VALIDATION_MODE option validates data without loading it and returns the first n rows that would be loaded?

- A) RETURN_ERRORS
- B) RETURN_n_ROWS
- C) RETURN_ALL_ERRORS
- D) VALIDATE_n_ROWS

**Answer: B)**

**Explanation:** VALIDATION_MODE = RETURN_n_ROWS (e.g., RETURN_5_ROWS) validates and returns the specified number of rows without loading any data. This helps verify that the data format and mapping are correct before doing the actual load.

---

### Question 15
What does the PURGE option do in a COPY INTO statement?

- A) Deletes the target table before loading
- B) Removes the data files from the stage after successful loading
- C) Truncates the stage after loading
- D) Clears the load metadata history

**Answer: B)**

**Explanation:** When PURGE = TRUE is set in a COPY INTO statement, Snowflake automatically removes (purges) the data files from the stage after the data is loaded successfully. The default is PURGE = FALSE.

---

### Question 16
A company is loading multiple CSV files from a stage but only wants to load files matching a specific naming pattern. Which COPY INTO option should they use?

- A) FILE_FORMAT
- B) FILES
- C) PATTERN
- D) FILTER

**Answer: C)**

**Explanation:** The PATTERN option accepts a regular expression to filter which files to load from a stage. For example, PATTERN = '.*sales.*[.]csv' loads only files with "sales" in the name. The FILES option takes an explicit list of file names, while PATTERN uses regex matching.

---

### Question 17
What is the SIZE_LIMIT option used for in COPY INTO?

- A) Limits the size of individual records
- B) Specifies the maximum number of rows to load
- C) Specifies the maximum aggregate size (in bytes) of data files to load
- D) Limits the size of the target table

**Answer: C)**

**Explanation:** SIZE_LIMIT specifies the maximum aggregate size (in bytes) of data files to include in a load operation. Once the threshold is met, the COPY operation stops loading. This is useful for breaking large loads into manageable chunks. Note: it does not split files — it includes whole files until the threshold is exceeded.

---

### Question 18
Your COPY INTO command successfully loaded a file yesterday. Today you run the same command again without changes. What happens?

- A) The data is loaded again, creating duplicates
- B) The command fails with an error
- C) The file is skipped because it was already loaded (load metadata)
- D) The file is compared row-by-row and only new rows are loaded

**Answer: C)**

**Explanation:** Snowflake maintains load metadata for 64 days that tracks which files have been loaded. If the same file (same name and checksum) is loaded again, COPY INTO skips it by default to prevent duplicate data. Use FORCE = TRUE to override this behavior.

---

### Question 19
How long does Snowflake retain COPY INTO load metadata by default?

- A) 7 days
- B) 14 days
- C) 64 days
- D) 90 days

**Answer: C)**

**Explanation:** Snowflake retains load metadata for 64 days. After 64 days, the metadata expires and COPY INTO may reload previously loaded files if they are still in the stage. This is important for data pipeline design.

---

### Question 20
Which option forces Snowflake to reload data files that have already been loaded?

- A) RELOAD = TRUE
- B) FORCE = TRUE
- C) OVERWRITE = TRUE
- D) IGNORE_METADATA = TRUE

**Answer: B)**

**Explanation:** FORCE = TRUE in COPY INTO forces loading of all files regardless of load history metadata. This means even files that were previously loaded successfully will be loaded again, potentially causing duplicates.

---

### Question 21
A data engineer runs COPY INTO and receives an error about a data type mismatch. They want to load all valid rows and skip the problematic ones. Which ON_ERROR value should they use?

- A) ABORT_STATEMENT
- B) SKIP_FILE
- C) CONTINUE
- D) SKIP_ROW

**Answer: C)**

**Explanation:** ON_ERROR = CONTINUE instructs Snowflake to continue loading and skip any rows that cause errors. It loads all valid rows and reports the number of errors after the operation completes. There is no SKIP_ROW option — CONTINUE provides this behavior at the row level.

---

### Question 22
Which of the following statements about the COPY INTO command is FALSE?

- A) COPY INTO can load data from external stages
- B) COPY INTO supports transformation of data during load using SELECT
- C) COPY INTO can perform joins between staged files during loading
- D) COPY INTO can load data from internal stages

**Answer: C)**

**Explanation:** COPY INTO supports loading from internal and external stages, and it supports column reordering, omission, casting, and simple transformations using a SELECT statement. However, it cannot perform joins between files during loading. For complex transformations, data must be loaded first and then transformed.

---

### Question 23
What happens when you use COPY INTO with VALIDATION_MODE set and also specify ON_ERROR?

- A) Both options work together
- B) VALIDATION_MODE takes precedence
- C) ON_ERROR takes precedence
- D) VALIDATION_MODE cannot be used with ON_ERROR — it returns an error

**Answer: D)**

**Explanation:** VALIDATION_MODE and ON_ERROR are mutually exclusive. VALIDATION_MODE is for dry-run validation without loading, while ON_ERROR controls behavior during actual loading. Specifying both produces an error.

---

## Snowpipe

### Question 24
What is Snowpipe?

- A) A data transformation pipeline tool
- B) A continuous data ingestion service that loads data automatically as files arrive in a stage
- C) A scheduled batch loading mechanism
- D) A real-time streaming API

**Answer: B)**

**Explanation:** Snowpipe is Snowflake's continuous data ingestion service. It loads data within minutes of files arriving in a stage, using a serverless compute model. It enables near-real-time loading without requiring a running virtual warehouse.

---

### Question 25
How does Snowpipe auto-ingest work with Amazon S3?

- A) Snowpipe polls S3 every minute
- B) S3 event notifications trigger an SQS queue that Snowpipe monitors
- C) A Snowflake task checks for new files
- D) AWS Lambda invokes Snowpipe directly

**Answer: B)**

**Explanation:** For auto-ingest with S3, you configure S3 event notifications to send messages to an Amazon SQS queue managed by Snowflake. When new files arrive, the event notification triggers Snowpipe to load the data. This is event-driven, not polling-based.

---

### Question 26
How is Snowpipe billed?

- A) Per-warehouse credit consumption based on warehouse size
- B) Per-file overhead charge plus compute time based on serverless compute resources consumed
- C) Flat monthly fee per pipe
- D) Only for the storage of queued files

**Answer: B)**

**Explanation:** Snowpipe uses a serverless compute model and is billed based on the actual compute resources consumed to load data, measured per-second. There is also a small per-file overhead charge (0.06 credits per 1000 files). It does not use a customer's virtual warehouse.

---

### Question 27
Which statement about Snowpipe is TRUE?

- A) Snowpipe uses the customer's virtual warehouse for compute
- B) Snowpipe guarantees exactly-once delivery
- C) Snowpipe loads are performed using Snowflake-managed compute resources
- D) Snowpipe requires a TASK to be scheduled

**Answer: C)**

**Explanation:** Snowpipe uses Snowflake-managed serverless compute resources, not the customer's virtual warehouses. It provides "at least once" semantics (not exactly-once) through load metadata tracking. It does not require tasks — it operates independently.

---

### Question 28
A company notices their Snowpipe costs are high despite loading small amounts of data. They load 100,000 small files (1 KB each) per day. What is the likely cause?

- A) The warehouse size is too large
- B) Per-file overhead charges are accumulating due to the large number of small files
- C) Network transfer costs
- D) Stage storage costs

**Answer: B)**

**Explanation:** Snowpipe charges approximately 0.06 credits per 1000 files as overhead. With 100,000 files per day, that's 6 credits/day in overhead alone, regardless of file size. The best practice is to batch small files into larger ones (100-250 MB) to reduce per-file overhead.

---

### Question 29
What is the recommended approach to trigger Snowpipe loading without cloud event notifications?

- A) Use a Snowflake TASK
- B) Call the Snowpipe REST API (insertFiles endpoint)
- C) Use a PUT command
- D) Use a CRON job with COPY INTO

**Answer: B)**

**Explanation:** When auto-ingest via cloud event notifications is not suitable, you can use the Snowpipe REST API's insertFiles endpoint to explicitly notify Snowpipe about new files to load. This gives programmatic control over when files are queued for loading.

---

### Question 30
What SQL command is used to check the load history of a Snowpipe?

- A) SHOW PIPE STATUS
- B) SELECT * FROM TABLE(INFORMATION_SCHEMA.PIPE_USAGE_HISTORY())
- C) SELECT * FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY())
- D) DESCRIBE PIPE

**Answer: C)**

**Explanation:** The COPY_HISTORY table function in INFORMATION_SCHEMA returns the loading history for both COPY INTO and Snowpipe operations. You can also use SYSTEM$PIPE_STATUS() for current pipe status, but for historical load details, COPY_HISTORY is the appropriate function.

---

### Question 31
Which statement creates a Snowpipe with auto-ingest enabled?

- A) CREATE PIPE my_pipe AS COPY INTO my_table FROM @my_stage AUTO_INGEST = TRUE;
- B) CREATE PIPE my_pipe AUTO_INGEST = TRUE AS COPY INTO my_table FROM @my_stage;
- C) CREATE PIPE my_pipe AS COPY INTO my_table FROM @my_stage WITH AUTO_INGEST;
- D) CREATE PIPE my_pipe SCHEDULE = 'AUTO' AS COPY INTO my_table FROM @my_stage;

**Answer: B)**

**Explanation:** The correct syntax places AUTO_INGEST = TRUE as a pipe property before the AS keyword. The pipe definition contains the COPY INTO statement that defines the source stage and target table.

---

### Question 32
A Snowpipe is loading data from an external stage. A file is loaded, then deleted from the stage and re-uploaded with different content but the same name. What happens?

- A) Snowpipe reloads the file automatically
- B) Snowpipe skips the file due to load metadata
- C) Snowpipe detects the content change and loads the new version
- D) An error is thrown

**Answer: B)**

**Explanation:** Snowpipe tracks loaded files by name in its metadata (for 14 days for Snowpipe specifically). If a file with the same name is re-uploaded, Snowpipe considers it already loaded and skips it, even if the content changed. To reload, you must use a different file name or use the REST API to resolve the issue.

---

## File Formats

### Question 33
Which file formats are supported by Snowflake for data loading? (Choose the best answer)

- A) CSV, JSON, Avro, ORC, Parquet, XML
- B) CSV, JSON, Parquet only
- C) CSV, JSON, Avro, ORC, Parquet, XML, XLSX
- D) CSV, TSV, JSON, YAML, Parquet

**Answer: A)**

**Explanation:** Snowflake supports loading from CSV, JSON, Avro, ORC, Parquet, and XML file formats. It does not natively support XLSX, YAML, or other formats. Semi-structured formats (JSON, Avro, ORC, Parquet, XML) can be loaded into VARIANT columns.

---

### Question 34
What is the purpose of a named file format object in Snowflake?

- A) To encrypt files during transfer
- B) To define a reusable set of format options for parsing data files
- C) To compress files before staging
- D) To validate data quality rules

**Answer: B)**

**Explanation:** A named file format object stores a set of parsing and formatting options (like field delimiter, skip header, date format) that can be reused across multiple COPY INTO statements and stage definitions, promoting consistency and reducing repetition.

---

### Question 35
When loading a CSV file, which option specifies that the first row contains column headers and should be skipped?

- A) FIRST_ROW_AS_HEADER = TRUE
- B) SKIP_HEADER = 1
- C) HEADER = TRUE
- D) IGNORE_FIRST_ROW = TRUE

**Answer: B)**

**Explanation:** The SKIP_HEADER option in the CSV file format specifies the number of header lines to skip at the beginning of each file. SKIP_HEADER = 1 skips the first row. There is also PARSE_HEADER = TRUE which uses the first row as column names.

---

### Question 36
A data engineer needs to load a JSON file where each line contains a separate JSON object. Which file format option is relevant?

- A) STRIP_OUTER_ARRAY = TRUE
- B) ENABLE_OCTAL = TRUE
- C) ALLOW_DUPLICATE = TRUE
- D) No special option needed — Snowflake handles NDJSON by default

**Answer: D)**

**Explanation:** Snowflake natively handles NDJSON (newline-delimited JSON) where each line is a separate JSON object. No special option is needed. STRIP_OUTER_ARRAY is used when the JSON file contains an outer array wrapping multiple objects (e.g., [{...},{...}]).

---

### Question 37
What does STRIP_OUTER_ARRAY = TRUE do when loading JSON data?

- A) Removes the outer brackets from each JSON object
- B) Splits a JSON array into separate rows, one per array element
- C) Removes nested arrays from the data
- D) Flattens all arrays in the JSON

**Answer: B)**

**Explanation:** STRIP_OUTER_ARRAY = TRUE instructs Snowflake to remove the outer array brackets and load each element of the array as a separate row in the target table. This is useful when a JSON file contains a single array with multiple objects: [{obj1},{obj2},...].

---

### Question 38
Which compression format is NOT supported by Snowflake for data loading?

- A) GZIP
- B) BZIP2
- C) ZSTD
- D) RAR

**Answer: D)**

**Explanation:** Snowflake supports GZIP, BZ2 (BZIP2), DEFLATE, RAW_DEFLATE, ZSTD, BROTLI, LZO (for Hadoop), and SNAPPY compression. RAR is not supported.

---

### Question 39
When loading Parquet files, where is the data stored in the target table?

- A) Automatically mapped to corresponding table columns by name
- B) Always in a single VARIANT column unless a query is specified
- C) In a BINARY column
- D) Parquet files cannot be loaded directly

**Answer: B)**

**Explanation:** By default, loading Parquet (and other semi-structured formats) without a SELECT places all data in a single VARIANT column (commonly named $1). To map Parquet columns to table columns, you use a SELECT statement in the COPY INTO command with column expressions like $1:column_name::type.

---

### Question 40
What is the default field delimiter for CSV files in Snowflake?

- A) Tab (\t)
- B) Pipe (|)
- C) Comma (,)
- D) Semicolon (;)

**Answer: C)**

**Explanation:** The default FIELD_DELIMITER for CSV file format in Snowflake is comma (,). This can be changed to any single character or multi-character string using the FIELD_DELIMITER option.

---

## Data Unloading

### Question 41
Which SQL command is used to unload data from a Snowflake table to a stage?

- A) EXPORT INTO
- B) UNLOAD INTO
- C) COPY INTO @stage FROM table
- D) PUT table TO @stage

**Answer: C)**

**Explanation:** Data unloading uses the COPY INTO command with the stage as the target: COPY INTO @my_stage FROM my_table. The same COPY INTO command is used for both loading (stage→table) and unloading (table→stage), differentiated by the direction.

---

### Question 42
What is the default file format when unloading data from Snowflake?

- A) JSON
- B) Parquet
- C) CSV (with comma delimiter)
- D) TSV

**Answer: C)**

**Explanation:** The default file format for unloading data is CSV with comma as the field delimiter. You can specify different formats (JSON, Parquet, etc.) using the FILE_FORMAT option in the COPY INTO statement.

---

### Question 43
When unloading data, what does the SINGLE = TRUE option do?

- A) Exports only one row per file
- B) Creates a single output file instead of multiple parallel files
- C) Loads data to only one node
- D) Restricts output to one column

**Answer: B)**

**Explanation:** SINGLE = TRUE produces a single output file from the unload operation instead of the default behavior of creating multiple files in parallel. Note that this may impact performance for large datasets since parallelism is reduced.

---

### Question 44
A company needs to unload data to their own S3 bucket while partitioning the output by date. Which feature helps organize unloaded files into a folder structure?

- A) PARTITION BY expression in COPY INTO
- B) GROUP BY in the SELECT
- C) Creating separate stages per partition
- D) Using multiple COPY INTO statements

**Answer: A)**

**Explanation:** The PARTITION BY option in COPY INTO (unload) organizes output files into a directory structure based on column expressions. For example, PARTITION BY TO_DATE(created_at) creates date-based folder partitions in the stage.

---

### Question 45
What is the default maximum file size for unloaded files?

- A) 16 MB
- B) 64 MB
- C) 128 MB
- D) 256 MB

**Answer: A)**

**Explanation:** The default MAX_FILE_SIZE for data unloading is 16 MB (16777216 bytes). This can be adjusted to create larger or smaller output files. Snowflake splits output across multiple files for parallelism.

---

### Question 46
When unloading data to Parquet format, which statement is TRUE?

- A) VARIANT columns cannot be unloaded to Parquet
- B) The HEADER option is required
- C) Column names and types are preserved in the Parquet file metadata
- D) Parquet unloading requires a warehouse size of MEDIUM or larger

**Answer: C)**

**Explanation:** When unloading to Parquet format, Snowflake preserves column names and data types in the Parquet file metadata (schema). This makes the unloaded files self-describing and easy to consume by other tools that support Parquet.

---

### Question 47
You need to unload query results (not a full table) to a stage. Which approach is correct?

- A) COPY INTO @my_stage FROM (SELECT col1, col2 FROM my_table WHERE status = 'active');
- B) SELECT col1, col2 INTO @my_stage FROM my_table WHERE status = 'active';
- C) EXPORT (SELECT col1, col2 FROM my_table) TO @my_stage;
- D) INSERT INTO @my_stage SELECT col1, col2 FROM my_table;

**Answer: A)**

**Explanation:** COPY INTO @stage FROM (query) allows you to unload the results of any query, not just a full table. The query can include filters, joins, aggregations, and column transformations.

---

## Connectors and Drivers

### Question 48
Which of the following is NOT an official Snowflake connector or driver?

- A) Snowflake Connector for Python
- B) Snowflake JDBC Driver
- C) Snowflake Connector for Apache Hive
- D) Snowflake ODBC Driver

**Answer: C)**

**Explanation:** Snowflake provides official connectors/drivers for Python, JDBC, ODBC, Node.js, Go, .NET, PHP PDO, Spark, and Kafka. There is no official Snowflake Connector for Apache Hive. Hive integration would typically go through Spark or external tables.

---

### Question 49
What is the Snowflake Connector for Kafka used for?

- A) Loading data from Kafka topics into Snowflake tables
- B) Exporting Snowflake data to Kafka topics
- C) Managing Kafka clusters from Snowflake
- D) Streaming query results to Kafka consumers

**Answer: A)**

**Explanation:** The Snowflake Connector for Kafka (Kafka connector / Kafka Sink) reads data from Kafka topics and loads it into Snowflake tables. It acts as a Kafka sink connector, supporting both Snowpipe and Snowpipe Streaming ingestion methods. It is unidirectional (Kafka → Snowflake).

---

### Question 50
Which connector allows Spark applications to read from and write to Snowflake?

- A) Snowflake JDBC Driver
- B) Snowflake Connector for Spark
- C) Snowflake Connector for Python
- D) SnowSQL

**Answer: B)**

**Explanation:** The Snowflake Connector for Spark enables bidirectional data transfer between Spark DataFrames and Snowflake tables. It pushes query processing to Snowflake when possible and uses an optimized internal stage for data transfer.

---

### Question 51
A data engineer is setting up a Python application to connect to Snowflake. Which authentication method is NOT supported by the Python connector?

- A) Username/password
- B) Key-pair authentication
- C) OAuth
- D) SAML browser-based SSO through the connector itself in a headless server environment

**Answer: D)**

**Explanation:** The Snowflake Python connector supports username/password, key-pair authentication, OAuth, and externalbrowser (SSO) authentication. However, externalbrowser/SSO requires a browser and cannot work in headless server environments. For headless environments, key-pair or OAuth is recommended.

---

### Question 52
What is SnowSQL?

- A) A web-based SQL editor
- B) Snowflake's command-line client for executing SQL and performing PUT/GET operations
- C) A stored procedure language
- D) An ODBC driver wrapper

**Answer: B)**

**Explanation:** SnowSQL is Snowflake's official command-line interface (CLI) tool. It allows users to execute SQL queries, DDL/DML statements, and is the primary tool for PUT and GET commands to upload/download files from stages.

---

## Advanced / Scenario-Based Questions

### Question 53
A retail company receives hourly sales files from 500 stores. Each file is approximately 500 KB. They use Snowpipe auto-ingest. After analysis, they find Snowpipe overhead costs are higher than expected. What is the best optimization?

- A) Increase the warehouse size
- B) Batch store files into larger aggregate files before staging (e.g., combine stores into one file per hour)
- C) Switch from Snowpipe to a scheduled COPY INTO task running every hour
- D) Reduce the number of stores sending data

**Answer: B)**

**Explanation:** With 500 files/hour (12,000/day), the per-file overhead (0.06 credits/1000 files) is significant for small files. Batching files into fewer, larger files reduces the per-file overhead while maintaining near-real-time loading. Alternatively, option C could work but would sacrifice the near-real-time benefit.

---

### Question 54
A COPY INTO command loads 10 files. Files 1-7 load successfully, but file 8 has errors. The command uses ON_ERROR = 'ABORT_STATEMENT'. What is the final state?

- A) Files 1-7 are loaded; files 8-10 are skipped
- B) No data is loaded — the entire operation is rolled back
- C) Files 1-8 fail; files 9-10 are not attempted
- D) All files are loaded with error rows skipped

**Answer: B)**

**Explanation:** With ON_ERROR = 'ABORT_STATEMENT', if any error is encountered, the entire COPY INTO operation is aborted and rolled back. No data from any file (including the 7 successful ones) is committed. This is the default and safest behavior for data integrity.

---

### Question 55
A team has a Snowpipe configured on an external stage. They modify the COPY INTO definition in the pipe (ALTER PIPE ... SET ...). What must they do after the modification?

- A) Nothing — changes take effect immediately
- B) Pause and resume the pipe using ALTER PIPE ... SET PIPE_EXECUTION_PAUSED = TRUE/FALSE
- C) Drop and recreate the pipe
- D) Restart the Snowflake warehouse

**Answer: C)**

**Explanation:** The COPY statement in a pipe definition cannot be modified in place. To change the COPY INTO statement (e.g., add transformations, change the target table), you must drop and recreate the pipe. You can use ALTER PIPE to change pipe properties (like comments), but not the COPY definition itself. Alternatively, CREATE OR REPLACE PIPE achieves this in one step.

---

### Question 56
You are loading CSV files with the COPY INTO command. Some files have 10 columns but your target table has 8 columns. What happens with default settings?

- A) The extra columns are automatically ignored
- B) The load fails with a column mismatch error
- C) Extra columns are loaded into a VARIANT overflow column
- D) The table is automatically altered to add columns

**Answer: B)**

**Explanation:** By default, the number of columns in the data file must match the target table. A mismatch causes an error. To handle this, you can use a SELECT with column specifications in the COPY INTO (e.g., SELECT $1, $2, ..., $8 FROM @stage) to explicitly map which columns to load, or set ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE.

---

### Question 57
An organization uses Snowflake on AWS. They want their external stage to use the same IAM role for both Snowpipe and interactive COPY INTO. What is the recommended approach?

- A) Create two separate stages with the same credentials
- B) Use a storage integration object referenced by the stage
- C) Embed AWS keys directly in the stage definition
- D) Use a network policy to share credentials

**Answer: B)**

**Explanation:** A storage integration provides a centralized, secure way to manage cloud storage access. Both Snowpipe and COPY INTO can use the same stage (which references the storage integration), ensuring consistent access control without embedding credentials. This also enables IAM role-based access.

---

### Question 58
What function can you use to validate previously loaded data and get error details after a COPY INTO operation completes?

- A) VALIDATE()
- B) COPY_HISTORY()
- C) LOAD_ERRORS()
- D) GET_ERRORS()

**Answer: A)**

**Explanation:** The VALIDATE() function allows you to view all errors encountered during a previous COPY INTO execution. It takes the query ID of the COPY INTO statement and returns the error details. Syntax: SELECT * FROM TABLE(VALIDATE(table_name, JOB_ID => 'query_id')).

---

### Question 59
A data pipeline loads Parquet files into a Snowflake table. The engineer wants to capture the source file name and row number for each loaded record. Which metadata columns should they use?

- A) METADATA$FILENAME and METADATA$FILE_ROW_NUMBER
- B) $FILE_NAME and $ROW_NUM
- C) SOURCE_FILE() and ROW_NUMBER()
- D) STAGE$FILENAME and STAGE$ROW_NUMBER

**Answer: A)**

**Explanation:** Snowflake provides metadata columns that can be referenced in COPY INTO SELECT statements: METADATA$FILENAME (source file name), METADATA$FILE_ROW_NUMBER (row number within the file), METADATA$FILE_CONTENT_KEY (checksum), and METADATA$FILE_LAST_MODIFIED. These enable data lineage tracking.

---

### Question 60
A company's data pipeline requires loading files larger than 5 GB. They are using an internal stage. What should they be aware of?

- A) Files larger than 5 GB cannot be loaded into Snowflake
- B) PUT automatically splits files into chunks for upload; the large file can be loaded, but smaller files (100-250 MB) are recommended for better COPY INTO parallelism
- C) They must use an external stage for files over 5 GB
- D) A special license is required for large file support

**Answer: B)**

**Explanation:** PUT can handle large files by automatically splitting them into chunks for parallel upload. However, for COPY INTO performance, Snowflake recommends splitting source files into 100-250 MB compressed chunks before staging. Large single files reduce loading parallelism because only CSV files can be split by COPY INTO; semi-structured formats load as whole files.

---

### Question 61
Your team loads data using COPY INTO with MATCH_BY_COLUMN_NAME = 'CASE_INSENSITIVE'. What does this enable?

- A) Columns in the file are matched to table columns by name rather than position
- B) All column names are converted to uppercase before loading
- C) The file's column order must match the table's column order
- D) Only columns with matching cases are loaded

**Answer: A)**

**Explanation:** MATCH_BY_COLUMN_NAME = 'CASE_INSENSITIVE' (or 'CASE_SENSITIVE') matches columns in semi-structured data files (Parquet, Avro, ORC) to the target table columns by name rather than ordinal position. This is useful when column ordering differs between files and the table.

---

### Question 62
A Snowflake account has a pipe defined with AUTO_INGEST = TRUE on Azure. What Azure service is used for event notifications?

- A) Azure Functions
- B) Azure Event Grid
- C) Azure Service Bus
- D) Azure Logic Apps

**Answer: B)**

**Explanation:** On Microsoft Azure, Snowpipe auto-ingest uses Azure Event Grid to deliver blob storage event notifications (blob created events) to Snowflake. This triggers Snowpipe to load newly arrived files from Azure Blob Storage or Azure Data Lake Storage.

---

### Question 63
A data engineer accidentally loads the wrong file. They want to identify which files were loaded in the last hour. Which approach is best?

- A) SELECT * FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY(TABLE_NAME=>'my_table', START_TIME=>DATEADD(HOUR, -1, CURRENT_TIMESTAMP())))
- B) SHOW LOADS ON my_table
- C) SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.LOAD_HISTORY WHERE LAST_HOUR = TRUE
- D) LIST @my_stage

**Answer: A)**

**Explanation:** The INFORMATION_SCHEMA.COPY_HISTORY() table function returns detailed information about data loaded via COPY INTO (and Snowpipe) including file names, row counts, errors, and timestamps. It accepts time range parameters to filter results.

---

### Question 64
Which statement about loading data into VARIANT columns is TRUE?

- A) VARIANT columns have a maximum size of 16 MB per value
- B) VARIANT columns can only store JSON data
- C) Individual VARIANT values cannot exceed 16 MB in compressed size
- D) VARIANT columns do not support indexing or querying nested elements

**Answer: A)**

**Explanation:** Individual VARIANT values have a maximum uncompressed size limit of 16 MB. VARIANT columns can store any semi-structured data (JSON, Avro, ORC, Parquet, XML). They support dot notation and bracket notation for querying nested elements and can be used with FLATTEN for denormalization.

---

### Question 65
A financial services company needs to unload sensitive customer data to a partner's S3 bucket. The data must be encrypted. What encryption does Snowflake provide for unloaded files to external stages?

- A) No encryption is applied by default
- B) Snowflake always applies 256-bit AES encryption with a customer-managed key
- C) Client-side encryption can be configured on the stage using a master key
- D) Encryption is only available for internal stages

**Answer: C)**

**Explanation:** For external stages, you can configure client-side encryption by specifying a MASTER_KEY in the stage's encryption settings. This encrypts files before they leave Snowflake. For internal stages, files are automatically encrypted with 128-bit or 256-bit keys. The ENCRYPTION option on the stage definition controls this behavior.

---
