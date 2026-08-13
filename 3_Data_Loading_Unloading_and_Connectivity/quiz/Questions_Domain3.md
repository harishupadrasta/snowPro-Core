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

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The PUT command is used to upload (stage) files from a local file system to a Snowflake internal stage (named, table, or user stage). PUT is only available through SnowSQL or the JDBC/ODBC drivers, not through the Snowflake web UI.

</details>

---

### Question 2
Which of the following is NOT a type of internal stage in Snowflake?

- A) User stage
- B) Table stage
- C) Named stage
- D) Schema stage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**

**Explanation:** Snowflake supports three types of internal stages: User stages (@~), Table stages (@%table_name), and Named stages (@stage_name). There is no "Schema stage" type in Snowflake.

</details>

---

### Question 3
What is the default prefix for referencing a user stage?

- A) @%
- B) @~
- C) @$
- D) @user/

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The user stage is referenced with @~ (at-tilde). Each user has their own stage automatically. Table stages use @%table_name, and named stages use @stage_name.

</details>

---

### Question 4
A company stores its data files in an Amazon S3 bucket. Which type of Snowflake stage should they create to reference this location?

- A) Internal named stage
- B) External stage
- C) Table stage
- D) User stage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** An external stage references data files stored in a location outside of Snowflake, such as Amazon S3, Google Cloud Storage, or Microsoft Azure Blob Storage. You create an external stage with CREATE STAGE specifying the URL and credentials.

</details>

---

### Question 5
Which command downloads files from a Snowflake internal stage to the local file system?

- A) DOWNLOAD
- B) COPY FROM
- C) GET
- D) EXPORT

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The GET command downloads data files from a Snowflake internal stage to the local file system. Like PUT, GET is only available through SnowSQL or the JDBC/ODBC drivers.

</details>

---

### Question 6
A data engineer needs to list files in a stage. Which command should they use?

- A) SHOW FILES IN @my_stage
- B) LIST @my_stage
- C) DESCRIBE STAGE @my_stage
- D) SELECT * FROM @my_stage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The LIST (or LS) command lists the files in a stage. DESCRIBE STAGE shows the stage properties but not the files. SELECT from a stage would attempt to query the data content, not list files.

</details>

---

### Question 7
What happens by default when you PUT a file that already exists in the internal stage?

- A) The file is overwritten
- B) The upload fails with an error
- C) A duplicate is created with a suffix
- D) The file is skipped (not uploaded)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** By default, the PUT command sets OVERWRITE=FALSE, which means the file is NOT uploaded if it already exists. However, if OVERWRITE=TRUE is set, the file is overwritten. Note: The actual default is OVERWRITE=FALSE, meaning files are skipped if they already exist. Let me correct: By default (OVERWRITE=FALSE), if the file already exists with the same name, the PUT command skips the upload. The correct answer should reflect that the default is to skip. Actually, the default behavior is that PUT will upload and overwrite the file. Let me verify: The PUT default for OVERWRITE is FALSE — the file is NOT uploaded if it already exists. Correct answer is D.

**Correction - Answer: D)**

**Explanation:** By default, PUT has OVERWRITE=FALSE. If a file with the same name already exists in the stage, the upload is skipped. Set OVERWRITE=TRUE to replace existing files.

</details>

---

### Question 8
Which of the following is TRUE about table stages?

- A) They can be altered or dropped independently
- B) They support file format options in the stage definition
- C) Each table has a stage automatically allocated to it
- D) They can be shared with other tables

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Every table in Snowflake automatically has a table stage associated with it (referenced as @%table_name). Table stages cannot be altered, dropped, or shared — they are tied to the table's lifecycle. They also do not support setting file format options in the stage definition.

</details>

---

### Question 9
Your organization requires that credentials for accessing cloud storage are managed centrally and not embedded in stage definitions. What Snowflake object should you use?

- A) Security integration
- B) Storage integration
- C) Network policy
- D) Resource monitor

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** A storage integration is a Snowflake object that stores a generated identity and access management (IAM) entity for external cloud storage, along with an optional set of allowed or blocked storage locations. This avoids embedding credentials directly in stage definitions.

</details>

---

### Question 10
What is the maximum file size recommended for data loading in Snowflake for optimal performance?

- A) 10-50 MB
- B) 100-250 MB compressed
- C) 1-5 GB
- D) No limit recommended

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** Snowflake recommends files be 100-250 MB in compressed size for optimal parallel loading. Files that are too small create overhead; files too large reduce parallelism because they cannot be split across warehouse nodes (except for CSV which can be split).

</details>

---

## COPY INTO Command Options

### Question 11
What is the default value of the ON_ERROR option in the COPY INTO command?

- A) CONTINUE
- B) ABORT_STATEMENT
- C) SKIP_FILE
- D) SKIP_FILE_5

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The default value of ON_ERROR for COPY INTO <table> is ABORT_STATEMENT, which aborts the entire load operation if any error is encountered in a data file. Other options include CONTINUE, SKIP_FILE, and SKIP_FILE_<num>|<num>%.

</details>

---

### Question 12
A data engineer wants to load data and skip any file that contains more than 10 errors. Which ON_ERROR setting should they use?

- A) ON_ERROR = SKIP_FILE
- B) ON_ERROR = SKIP_FILE_10
- C) ON_ERROR = CONTINUE
- D) ON_ERROR = ABORT_STATEMENT_10

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** ON_ERROR = SKIP_FILE_<num> skips a file when the number of errors in the file equals or exceeds the specified number. SKIP_FILE_10 will skip any file with 10 or more errors. SKIP_FILE skips files with any errors. CONTINUE loads all valid rows and skips individual error rows.

</details>

---

### Question 13
What does VALIDATION_MODE = 'RETURN_ERRORS' do?

- A) Loads data and returns errors in the result
- B) Validates all data without loading and returns all errors
- C) Validates the first 1000 rows without loading
- D) Loads data but logs errors to an error table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** VALIDATION_MODE = 'RETURN_ERRORS' validates the entire data file without loading any data and returns all errors found. This is useful for checking data quality before performing the actual load. Note that VALIDATION_MODE and COPY INTO cannot load data simultaneously — it is a dry-run validation only.

</details>

---

### Question 14
Which VALIDATION_MODE option validates data without loading it and returns the first n rows that would be loaded?

- A) RETURN_ERRORS
- B) RETURN_n_ROWS
- C) RETURN_ALL_ERRORS
- D) VALIDATE_n_ROWS

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** VALIDATION_MODE = RETURN_n_ROWS (e.g., RETURN_5_ROWS) validates and returns the specified number of rows without loading any data. This helps verify that the data format and mapping are correct before doing the actual load.

</details>

---

### Question 15
What does the PURGE option do in a COPY INTO statement?

- A) Deletes the target table before loading
- B) Removes the data files from the stage after successful loading
- C) Truncates the stage after loading
- D) Clears the load metadata history

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** When PURGE = TRUE is set in a COPY INTO statement, Snowflake automatically removes (purges) the data files from the stage after the data is loaded successfully. The default is PURGE = FALSE.

</details>

---

### Question 16
A company is loading multiple CSV files from a stage but only wants to load files matching a specific naming pattern. Which COPY INTO option should they use?

- A) FILE_FORMAT
- B) FILES
- C) PATTERN
- D) FILTER

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The PATTERN option accepts a regular expression to filter which files to load from a stage. For example, PATTERN = '.*sales.*[.]csv' loads only files with "sales" in the name. The FILES option takes an explicit list of file names, while PATTERN uses regex matching.

</details>

---

### Question 17
What is the SIZE_LIMIT option used for in COPY INTO?

- A) Limits the size of individual records
- B) Specifies the maximum number of rows to load
- C) Specifies the maximum aggregate size (in bytes) of data files to load
- D) Limits the size of the target table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** SIZE_LIMIT specifies the maximum aggregate size (in bytes) of data files to include in a load operation. Once the threshold is met, the COPY operation stops loading. This is useful for breaking large loads into manageable chunks. Note: it does not split files — it includes whole files until the threshold is exceeded.

</details>

---

### Question 18
Your COPY INTO command successfully loaded a file yesterday. Today you run the same command again without changes. What happens?

- A) The data is loaded again, creating duplicates
- B) The command fails with an error
- C) The file is skipped because it was already loaded (load metadata)
- D) The file is compared row-by-row and only new rows are loaded

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Snowflake maintains load metadata for 64 days that tracks which files have been loaded. If the same file (same name and checksum) is loaded again, COPY INTO skips it by default to prevent duplicate data. Use FORCE = TRUE to override this behavior.

</details>

---

### Question 19
How long does Snowflake retain COPY INTO load metadata by default?

- A) 7 days
- B) 14 days
- C) 64 days
- D) 90 days

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Snowflake retains load metadata for 64 days. After 64 days, the metadata expires and COPY INTO may reload previously loaded files if they are still in the stage. This is important for data pipeline design.

</details>

---

### Question 20
Which option forces Snowflake to reload data files that have already been loaded?

- A) RELOAD = TRUE
- B) FORCE = TRUE
- C) OVERWRITE = TRUE
- D) IGNORE_METADATA = TRUE

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** FORCE = TRUE in COPY INTO forces loading of all files regardless of load history metadata. This means even files that were previously loaded successfully will be loaded again, potentially causing duplicates.

</details>

---

### Question 21
A data engineer runs COPY INTO and receives an error about a data type mismatch. They want to load all valid rows and skip the problematic ones. Which ON_ERROR value should they use?

- A) ABORT_STATEMENT
- B) SKIP_FILE
- C) CONTINUE
- D) SKIP_ROW

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** ON_ERROR = CONTINUE instructs Snowflake to continue loading and skip any rows that cause errors. It loads all valid rows and reports the number of errors after the operation completes. There is no SKIP_ROW option — CONTINUE provides this behavior at the row level.

</details>

---

### Question 22
Which of the following statements about the COPY INTO command is FALSE?

- A) COPY INTO can load data from external stages
- B) COPY INTO supports transformation of data during load using SELECT
- C) COPY INTO can perform joins between staged files during loading
- D) COPY INTO can load data from internal stages

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** COPY INTO supports loading from internal and external stages, and it supports column reordering, omission, casting, and simple transformations using a SELECT statement. However, it cannot perform joins between files during loading. For complex transformations, data must be loaded first and then transformed.

</details>

---

### Question 23
What happens when you use COPY INTO with VALIDATION_MODE set and also specify ON_ERROR?

- A) Both options work together
- B) VALIDATION_MODE takes precedence
- C) ON_ERROR takes precedence
- D) VALIDATION_MODE cannot be used with ON_ERROR — it returns an error

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**

**Explanation:** VALIDATION_MODE and ON_ERROR are mutually exclusive. VALIDATION_MODE is for dry-run validation without loading, while ON_ERROR controls behavior during actual loading. Specifying both produces an error.

</details>

---

## Snowpipe

### Question 24
What is Snowpipe?

- A) A data transformation pipeline tool
- B) A continuous data ingestion service that loads data automatically as files arrive in a stage
- C) A scheduled batch loading mechanism
- D) A real-time streaming API

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** Snowpipe is Snowflake's continuous data ingestion service. It loads data within minutes of files arriving in a stage, using a serverless compute model. It enables near-real-time loading without requiring a running virtual warehouse.

</details>

---

### Question 25
How does Snowpipe auto-ingest work with Amazon S3?

- A) Snowpipe polls S3 every minute
- B) S3 event notifications trigger an SQS queue that Snowpipe monitors
- C) A Snowflake task checks for new files
- D) AWS Lambda invokes Snowpipe directly

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** For auto-ingest with S3, you configure S3 event notifications to send messages to an Amazon SQS queue managed by Snowflake. When new files arrive, the event notification triggers Snowpipe to load the data. This is event-driven, not polling-based.

</details>

---

### Question 26
How is Snowpipe billed?

- A) Per-warehouse credit consumption based on warehouse size
- B) Per-file overhead charge plus compute time based on serverless compute resources consumed
- C) Flat monthly fee per pipe
- D) Only for the storage of queued files

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** Snowpipe uses a serverless compute model and is billed based on the actual compute resources consumed to load data, measured per-second. There is also a small per-file overhead charge (0.06 credits per 1000 files). It does not use a customer's virtual warehouse.

</details>

---

### Question 27
Which statement about Snowpipe is TRUE?

- A) Snowpipe uses the customer's virtual warehouse for compute
- B) Snowpipe guarantees exactly-once delivery
- C) Snowpipe loads are performed using Snowflake-managed compute resources
- D) Snowpipe requires a TASK to be scheduled

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Snowpipe uses Snowflake-managed serverless compute resources, not the customer's virtual warehouses. It provides "at least once" semantics (not exactly-once) through load metadata tracking. It does not require tasks — it operates independently.

</details>

---

### Question 28
A company notices their Snowpipe costs are high despite loading small amounts of data. They load 100,000 small files (1 KB each) per day. What is the likely cause?

- A) The warehouse size is too large
- B) Per-file overhead charges are accumulating due to the large number of small files
- C) Network transfer costs
- D) Stage storage costs

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** Snowpipe charges approximately 0.06 credits per 1000 files as overhead. With 100,000 files per day, that's 6 credits/day in overhead alone, regardless of file size. The best practice is to batch small files into larger ones (100-250 MB) to reduce per-file overhead.

</details>

---

### Question 29
What is the recommended approach to trigger Snowpipe loading without cloud event notifications?

- A) Use a Snowflake TASK
- B) Call the Snowpipe REST API (insertFiles endpoint)
- C) Use a PUT command
- D) Use a CRON job with COPY INTO

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** When auto-ingest via cloud event notifications is not suitable, you can use the Snowpipe REST API's insertFiles endpoint to explicitly notify Snowpipe about new files to load. This gives programmatic control over when files are queued for loading.

</details>

---

### Question 30
What SQL command is used to check the load history of a Snowpipe?

- A) SHOW PIPE STATUS
- B) SELECT * FROM TABLE(INFORMATION_SCHEMA.PIPE_USAGE_HISTORY())
- C) SELECT * FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY())
- D) DESCRIBE PIPE

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The COPY_HISTORY table function in INFORMATION_SCHEMA returns the loading history for both COPY INTO and Snowpipe operations. You can also use SYSTEM$PIPE_STATUS() for current pipe status, but for historical load details, COPY_HISTORY is the appropriate function.

</details>

---

### Question 31
Which statement creates a Snowpipe with auto-ingest enabled?

- A) CREATE PIPE my_pipe AS COPY INTO my_table FROM @my_stage AUTO_INGEST = TRUE;
- B) CREATE PIPE my_pipe AUTO_INGEST = TRUE AS COPY INTO my_table FROM @my_stage;
- C) CREATE PIPE my_pipe AS COPY INTO my_table FROM @my_stage WITH AUTO_INGEST;
- D) CREATE PIPE my_pipe SCHEDULE = 'AUTO' AS COPY INTO my_table FROM @my_stage;

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The correct syntax places AUTO_INGEST = TRUE as a pipe property before the AS keyword. The pipe definition contains the COPY INTO statement that defines the source stage and target table.

</details>

---

### Question 32
A Snowpipe is loading data from an external stage. A file is loaded, then deleted from the stage and re-uploaded with different content but the same name. What happens?

- A) Snowpipe reloads the file automatically
- B) Snowpipe skips the file due to load metadata
- C) Snowpipe detects the content change and loads the new version
- D) An error is thrown

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** Snowpipe tracks loaded files by name in its metadata (for 14 days for Snowpipe specifically). If a file with the same name is re-uploaded, Snowpipe considers it already loaded and skips it, even if the content changed. To reload, you must use a different file name or use the REST API to resolve the issue.

</details>

---

## File Formats

### Question 33
Which file formats are supported by Snowflake for data loading? (Choose the best answer)

- A) CSV, JSON, Avro, ORC, Parquet, XML
- B) CSV, JSON, Parquet only
- C) CSV, JSON, Avro, ORC, Parquet, XML, XLSX
- D) CSV, TSV, JSON, YAML, Parquet

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** Snowflake supports loading from CSV, JSON, Avro, ORC, Parquet, and XML file formats. It does not natively support XLSX, YAML, or other formats. Semi-structured formats (JSON, Avro, ORC, Parquet, XML) can be loaded into VARIANT columns.

</details>

---

### Question 34
What is the purpose of a named file format object in Snowflake?

- A) To encrypt files during transfer
- B) To define a reusable set of format options for parsing data files
- C) To compress files before staging
- D) To validate data quality rules

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** A named file format object stores a set of parsing and formatting options (like field delimiter, skip header, date format) that can be reused across multiple COPY INTO statements and stage definitions, promoting consistency and reducing repetition.

</details>

---

### Question 35
When loading a CSV file, which option specifies that the first row contains column headers and should be skipped?

- A) FIRST_ROW_AS_HEADER = TRUE
- B) SKIP_HEADER = 1
- C) HEADER = TRUE
- D) IGNORE_FIRST_ROW = TRUE

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The SKIP_HEADER option in the CSV file format specifies the number of header lines to skip at the beginning of each file. SKIP_HEADER = 1 skips the first row. There is also PARSE_HEADER = TRUE which uses the first row as column names.

</details>

---

### Question 36
A data engineer needs to load a JSON file where each line contains a separate JSON object. Which file format option is relevant?

- A) STRIP_OUTER_ARRAY = TRUE
- B) ENABLE_OCTAL = TRUE
- C) ALLOW_DUPLICATE = TRUE
- D) No special option needed — Snowflake handles NDJSON by default

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**

**Explanation:** Snowflake natively handles NDJSON (newline-delimited JSON) where each line is a separate JSON object. No special option is needed. STRIP_OUTER_ARRAY is used when the JSON file contains an outer array wrapping multiple objects (e.g., [{...},{...}]).

</details>

---

### Question 37
What does STRIP_OUTER_ARRAY = TRUE do when loading JSON data?

- A) Removes the outer brackets from each JSON object
- B) Splits a JSON array into separate rows, one per array element
- C) Removes nested arrays from the data
- D) Flattens all arrays in the JSON

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** STRIP_OUTER_ARRAY = TRUE instructs Snowflake to remove the outer array brackets and load each element of the array as a separate row in the target table. This is useful when a JSON file contains a single array with multiple objects: [{obj1},{obj2},...].

</details>

---

### Question 38
Which compression format is NOT supported by Snowflake for data loading?

- A) GZIP
- B) BZIP2
- C) ZSTD
- D) RAR

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**

**Explanation:** Snowflake supports GZIP, BZ2 (BZIP2), DEFLATE, RAW_DEFLATE, ZSTD, BROTLI, LZO (for Hadoop), and SNAPPY compression. RAR is not supported.

</details>

---

### Question 39
When loading Parquet files, where is the data stored in the target table?

- A) Automatically mapped to corresponding table columns by name
- B) Always in a single VARIANT column unless a query is specified
- C) In a BINARY column
- D) Parquet files cannot be loaded directly

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** By default, loading Parquet (and other semi-structured formats) without a SELECT places all data in a single VARIANT column (commonly named $1). To map Parquet columns to table columns, you use a SELECT statement in the COPY INTO command with column expressions like $1:column_name::type.

</details>

---

### Question 40
What is the default field delimiter for CSV files in Snowflake?

- A) Tab (\t)
- B) Pipe (|)
- C) Comma (,)
- D) Semicolon (;)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The default FIELD_DELIMITER for CSV file format in Snowflake is comma (,). This can be changed to any single character or multi-character string using the FIELD_DELIMITER option.

</details>

---

## Data Unloading

### Question 41
Which SQL command is used to unload data from a Snowflake table to a stage?

- A) EXPORT INTO
- B) UNLOAD INTO
- C) COPY INTO @stage FROM table
- D) PUT table TO @stage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Data unloading uses the COPY INTO command with the stage as the target: COPY INTO @my_stage FROM my_table. The same COPY INTO command is used for both loading (stage→table) and unloading (table→stage), differentiated by the direction.

</details>

---

### Question 42
What is the default file format when unloading data from Snowflake?

- A) JSON
- B) Parquet
- C) CSV (with comma delimiter)
- D) TSV

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The default file format for unloading data is CSV with comma as the field delimiter. You can specify different formats (JSON, Parquet, etc.) using the FILE_FORMAT option in the COPY INTO statement.

</details>

---

### Question 43
When unloading data, what does the SINGLE = TRUE option do?

- A) Exports only one row per file
- B) Creates a single output file instead of multiple parallel files
- C) Loads data to only one node
- D) Restricts output to one column

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** SINGLE = TRUE produces a single output file from the unload operation instead of the default behavior of creating multiple files in parallel. Note that this may impact performance for large datasets since parallelism is reduced.

</details>

---

### Question 44
A company needs to unload data to their own S3 bucket while partitioning the output by date. Which feature helps organize unloaded files into a folder structure?

- A) PARTITION BY expression in COPY INTO
- B) GROUP BY in the SELECT
- C) Creating separate stages per partition
- D) Using multiple COPY INTO statements

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** The PARTITION BY option in COPY INTO (unload) organizes output files into a directory structure based on column expressions. For example, PARTITION BY TO_DATE(created_at) creates date-based folder partitions in the stage.

</details>

---

### Question 45
What is the default maximum file size for unloaded files?

- A) 16 MB
- B) 64 MB
- C) 128 MB
- D) 256 MB

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** The default MAX_FILE_SIZE for data unloading is 16 MB (16777216 bytes). This can be adjusted to create larger or smaller output files. Snowflake splits output across multiple files for parallelism.

</details>

---

### Question 46
When unloading data to Parquet format, which statement is TRUE?

- A) VARIANT columns cannot be unloaded to Parquet
- B) The HEADER option is required
- C) Column names and types are preserved in the Parquet file metadata
- D) Parquet unloading requires a warehouse size of MEDIUM or larger

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** When unloading to Parquet format, Snowflake preserves column names and data types in the Parquet file metadata (schema). This makes the unloaded files self-describing and easy to consume by other tools that support Parquet.

</details>

---

### Question 47
You need to unload query results (not a full table) to a stage. Which approach is correct?

- A) COPY INTO @my_stage FROM (SELECT col1, col2 FROM my_table WHERE status = 'active');
- B) SELECT col1, col2 INTO @my_stage FROM my_table WHERE status = 'active';
- C) EXPORT (SELECT col1, col2 FROM my_table) TO @my_stage;
- D) INSERT INTO @my_stage SELECT col1, col2 FROM my_table;

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** COPY INTO @stage FROM (query) allows you to unload the results of any query, not just a full table. The query can include filters, joins, aggregations, and column transformations.

</details>

---

## Connectors and Drivers

### Question 48
Which of the following is NOT an official Snowflake connector or driver?

- A) Snowflake Connector for Python
- B) Snowflake JDBC Driver
- C) Snowflake Connector for Apache Hive
- D) Snowflake ODBC Driver

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** Snowflake provides official connectors/drivers for Python, JDBC, ODBC, Node.js, Go, .NET, PHP PDO, Spark, and Kafka. There is no official Snowflake Connector for Apache Hive. Hive integration would typically go through Spark or external tables.

</details>

---

### Question 49
What is the Snowflake Connector for Kafka used for?

- A) Loading data from Kafka topics into Snowflake tables
- B) Exporting Snowflake data to Kafka topics
- C) Managing Kafka clusters from Snowflake
- D) Streaming query results to Kafka consumers

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** The Snowflake Connector for Kafka (Kafka connector / Kafka Sink) reads data from Kafka topics and loads it into Snowflake tables. It acts as a Kafka sink connector, supporting both Snowpipe and Snowpipe Streaming ingestion methods. It is unidirectional (Kafka → Snowflake).

</details>

---

### Question 50
Which connector allows Spark applications to read from and write to Snowflake?

- A) Snowflake JDBC Driver
- B) Snowflake Connector for Spark
- C) Snowflake Connector for Python
- D) SnowSQL

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** The Snowflake Connector for Spark enables bidirectional data transfer between Spark DataFrames and Snowflake tables. It pushes query processing to Snowflake when possible and uses an optimized internal stage for data transfer.

</details>

---

### Question 51
A data engineer is setting up a Python application to connect to Snowflake. Which authentication method is NOT supported by the Python connector?

- A) Username/password
- B) Key-pair authentication
- C) OAuth
- D) SAML browser-based SSO through the connector itself in a headless server environment

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**

**Explanation:** The Snowflake Python connector supports username/password, key-pair authentication, OAuth, and externalbrowser (SSO) authentication. However, externalbrowser/SSO requires a browser and cannot work in headless server environments. For headless environments, key-pair or OAuth is recommended.

</details>

---

### Question 52
What is SnowSQL?

- A) A web-based SQL editor
- B) Snowflake's command-line client for executing SQL and performing PUT/GET operations
- C) A stored procedure language
- D) An ODBC driver wrapper

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** SnowSQL is Snowflake's official command-line interface (CLI) tool. It allows users to execute SQL queries, DDL/DML statements, and is the primary tool for PUT and GET commands to upload/download files from stages.

</details>

---

## Advanced / Scenario-Based Questions

### Question 53
A retail company receives hourly sales files from 500 stores. Each file is approximately 500 KB. They use Snowpipe auto-ingest. After analysis, they find Snowpipe overhead costs are higher than expected. What is the best optimization?

- A) Increase the warehouse size
- B) Batch store files into larger aggregate files before staging (e.g., combine stores into one file per hour)
- C) Switch from Snowpipe to a scheduled COPY INTO task running every hour
- D) Reduce the number of stores sending data

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** With 500 files/hour (12,000/day), the per-file overhead (0.06 credits/1000 files) is significant for small files. Batching files into fewer, larger files reduces the per-file overhead while maintaining near-real-time loading. Alternatively, option C could work but would sacrifice the near-real-time benefit.

</details>

---

### Question 54
A COPY INTO command loads 10 files. Files 1-7 load successfully, but file 8 has errors. The command uses ON_ERROR = 'ABORT_STATEMENT'. What is the final state?

- A) Files 1-7 are loaded; files 8-10 are skipped
- B) No data is loaded — the entire operation is rolled back
- C) Files 1-8 fail; files 9-10 are not attempted
- D) All files are loaded with error rows skipped

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** With ON_ERROR = 'ABORT_STATEMENT', if any error is encountered, the entire COPY INTO operation is aborted and rolled back. No data from any file (including the 7 successful ones) is committed. This is the default and safest behavior for data integrity.

</details>

---

### Question 55
A team has a Snowpipe configured on an external stage. They modify the COPY INTO definition in the pipe (ALTER PIPE ... SET ...). What must they do after the modification?

- A) Nothing — changes take effect immediately
- B) Pause and resume the pipe using ALTER PIPE ... SET PIPE_EXECUTION_PAUSED = TRUE/FALSE
- C) Drop and recreate the pipe
- D) Restart the Snowflake warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** The COPY statement in a pipe definition cannot be modified in place. To change the COPY INTO statement (e.g., add transformations, change the target table), you must drop and recreate the pipe. You can use ALTER PIPE to change pipe properties (like comments), but not the COPY definition itself. Alternatively, CREATE OR REPLACE PIPE achieves this in one step.

</details>

---

### Question 56
You are loading CSV files with the COPY INTO command. Some files have 10 columns but your target table has 8 columns. What happens with default settings?

- A) The extra columns are automatically ignored
- B) The load fails with a column mismatch error
- C) Extra columns are loaded into a VARIANT overflow column
- D) The table is automatically altered to add columns

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** By default, the number of columns in the data file must match the target table. A mismatch causes an error. To handle this, you can use a SELECT with column specifications in the COPY INTO (e.g., SELECT $1, $2, ..., $8 FROM @stage) to explicitly map which columns to load, or set ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE.

</details>

---

### Question 57
An organization uses Snowflake on AWS. They want their external stage to use the same IAM role for both Snowpipe and interactive COPY INTO. What is the recommended approach?

- A) Create two separate stages with the same credentials
- B) Use a storage integration object referenced by the stage
- C) Embed AWS keys directly in the stage definition
- D) Use a network policy to share credentials

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** A storage integration provides a centralized, secure way to manage cloud storage access. Both Snowpipe and COPY INTO can use the same stage (which references the storage integration), ensuring consistent access control without embedding credentials. This also enables IAM role-based access.

</details>

---

## Bonus: Advanced Scenario Questions

### Question 66
A Snowpipe has been running reliably for 3 months. On day 15, a file called `sales_20250601.csv` was loaded successfully. On day 30 (15 days later), the same file is re-uploaded to the stage with updated content. What happens?

- A) Snowpipe detects the content change and reloads the file
- B) Snowpipe skips the file because the name matches its 14-day metadata — but wait, 15 days have passed so the metadata expired, meaning the file WILL be reloaded
- C) Snowpipe never reloads files regardless of time elapsed
- D) Snowpipe loads only the changed rows

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Snowpipe skips the file because the name matches its 14-day metadata — but wait, 15 days have passed so the metadata expired, meaning the file WILL be reloaded**

**Explanation:** Snowpipe retains load metadata for 14 days. After 14 days, the record that `sales_20250601.csv` was loaded is purged. If the file appears again (via re-upload or new event notification) after 14 days, Snowpipe treats it as a new file and loads it — potentially creating duplicates. This is different from COPY INTO's 64-day metadata.

**Exam Trap:** Snowpipe metadata = 14 days. COPY INTO metadata = 64 days. After expiry, re-loaded files cause duplicates.

</details>

---

### Question 67
A data engineer runs COPY INTO with VALIDATION_MODE = 'RETURN_ALL_ERRORS' and discovers 500 data quality issues. They fix the source file and want to load it. They remove VALIDATION_MODE and run COPY INTO again. The command reports "0 files loaded." Why?

- A) VALIDATION_MODE marked the file as loaded in the metadata
- B) VALIDATION_MODE does NOT load data BUT the file's metadata was still recorded — use FORCE = TRUE to override
- C) The fixed file has the same name and COPY INTO's load metadata tracks it, but VALIDATION_MODE doesn't record metadata — the issue is something else
- D) The file must be re-staged after validation

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) VALIDATION_MODE does NOT load data BUT the file's metadata was still recorded — use FORCE = TRUE to override**

**Explanation:** Even though VALIDATION_MODE doesn't actually load data, the file IS registered in the load metadata. Subsequent COPY INTO commands see it as "already processed." Use FORCE = TRUE to override the metadata check, or rename the file. This is a common trap in production pipelines.

**Exam Trap:** VALIDATION_MODE doesn't load data but DOES register the file in load metadata. You need FORCE = TRUE for the actual load.

</details>

---

### Question 68
A company's Snowpipe costs are $500/day. They load 50,000 files per day, each approximately 10KB. The actual data volume is only 500MB/day. What is the primary cost driver and fix?

- A) The warehouse is too large — downsize it
- B) Per-file overhead charges (0.06 credits per 1000 files × 50,000 files = 3 credits/day in overhead alone) — batch small files into fewer larger files before staging
- C) Network transfer costs from the high file count
- D) Stage storage costs

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Per-file overhead charges (0.06 credits per 1000 files × 50,000 files = 3 credits/day in overhead alone) — batch small files into fewer larger files before staging**

**Explanation:** Snowpipe charges ~0.06 credits per 1000 files as overhead regardless of file size. With 50,000 tiny files daily, the overhead alone is significant. The fix is to batch files into fewer, larger files (target 100-250MB each). Snowpipe uses serverless compute (no warehouse), so warehouse sizing is irrelevant.

**Exam Trap:** Snowpipe per-file overhead makes many small files expensive. Batch files to 100-250MB to minimize overhead costs.

</details>

---

### Question 69
A COPY INTO command with ON_ERROR = 'SKIP_FILE' loads 100 files. Files 1-50 load successfully. File 51 has a data error. Files 52-100 are error-free. What is the final result?

- A) Only files 1-50 are loaded; files 51-100 are all skipped
- B) Files 1-50 and 52-100 are loaded (99 files). Only file 51 is skipped
- C) All 100 files are loaded with error rows from file 51 silently dropped
- D) The entire operation is rolled back

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Files 1-50 and 52-100 are loaded (99 files). Only file 51 is skipped**

**Explanation:** SKIP_FILE skips only the specific file that contains errors — other error-free files continue loading normally. This differs from ABORT_STATEMENT (rolls back everything) and CONTINUE (loads all valid rows from all files including error files). SKIP_FILE provides a middle ground: skip bad files, load good files.

**Exam Trap:** SKIP_FILE = skip only the problematic file. ABORT_STATEMENT = roll back all files. CONTINUE = load valid rows from ALL files.

</details>

---

### Question 70
A team uses COPY INTO to load Parquet files daily. They want to track which source file each row came from. How should they modify their COPY INTO statement?

- A) Add a METADATA column to the target table and Snowflake auto-populates it
- B) Include METADATA$FILENAME in the SELECT clause of the COPY INTO command to load the source filename into a table column
- C) Query COPY_HISTORY after loading to join back the filename
- D) Parquet files automatically embed filename metadata

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Include METADATA$FILENAME in the SELECT clause of the COPY INTO command to load the source filename into a table column**

**Explanation:** COPY INTO with a SELECT clause can reference METADATA$FILENAME, METADATA$FILE_ROW_NUMBER, METADATA$FILE_CONTENT_KEY, and METADATA$FILE_LAST_MODIFIED. These are loaded into regular table columns for data lineage tracking. Example: `COPY INTO t FROM (SELECT $1:col1, METADATA$FILENAME FROM @stage)`.

**Exam Trap:** METADATA$ pseudo-columns are available ONLY during COPY INTO with a SELECT — they cannot be queried after loading unless stored explicitly.

</details>

---

### Question 71
An ETL pipeline uses COPY INTO with FORCE = TRUE because files are sometimes re-processed intentionally. The team discovers duplicate data in the target table. What is the root cause?

- A) FORCE = TRUE has a bug that doubles row counts
- B) FORCE = TRUE bypasses load metadata checking, so files that were already successfully loaded are loaded AGAIN, creating duplicates
- C) The files themselves contain duplicate rows
- D) Multiple warehouses are loading the same files concurrently

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) FORCE = TRUE bypasses load metadata checking, so files that were already successfully loaded are loaded AGAIN, creating duplicates**

**Explanation:** FORCE = TRUE tells COPY INTO to load ALL files regardless of whether they were previously loaded. If the same files remain in the stage and the command runs again, every file is re-loaded, creating exact duplicates. Use FORCE = TRUE only when you're certain you want to re-process, and combine it with PURGE = TRUE or manual file cleanup.

**Exam Trap:** FORCE = TRUE + files remaining in stage = guaranteed duplicates on next run. Always pair with PURGE or file management.

</details>

---

### Question 72
A data engineer creates a Snowpipe and immediately checks its status with SYSTEM$PIPE_STATUS(). The output shows `executionState: "RUNNING"` but no files are being loaded. What is the most likely explanation?

- A) The pipe is broken and needs recreation
- B) "RUNNING" means the pipe is active and listening — it's waiting for new files/notifications to arrive in the stage
- C) There's a connectivity issue with the cloud event notification
- D) The pipe needs to be explicitly started with ALTER PIPE ... RESUME

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) "RUNNING" means the pipe is active and listening — it's waiting for new files/notifications to arrive in the stage**

**Explanation:** SYSTEM$PIPE_STATUS() showing "RUNNING" simply means the pipe is active and ready to process files. It doesn't mean files are actively being loaded — it means the pipe is listening for notifications. Files must arrive in the stage (and trigger a notification) for loading to begin. This is normal operational state.

**Exam Trap:** Pipe "RUNNING" = healthy and waiting. It doesn't mean data is flowing — it means the pipe is ready to load when files arrive.

</details>

---

### Question 73
A company loads CSV files where some fields contain the delimiter character (comma) within quoted strings. They're getting column misalignment errors. What file format option resolves this?

- A) ESCAPE = '\\'
- B) FIELD_OPTIONALLY_ENCLOSED_BY = '"' (set the enclosure character to double-quote)
- C) FIELD_DELIMITER = '|' (switch delimiter)
- D) ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) FIELD_OPTIONALLY_ENCLOSED_BY = '"' (set the enclosure character to double-quote)**

**Explanation:** FIELD_OPTIONALLY_ENCLOSED_BY tells Snowflake that fields may be enclosed in quotes, and commas within quotes are part of the field value (not delimiters). This is the standard way to handle delimiters within data. ERROR_ON_COLUMN_COUNT_MISMATCH would just suppress the error without fixing the parsing.

**Exam Trap:** Quoted fields containing delimiters require FIELD_OPTIONALLY_ENCLOSED_BY — not escape characters or alternative delimiters.

</details>

---

### Question 74
A table stage (@%my_table) contains 50 files. A data engineer wants to load only files matching the pattern `sales_2025*.csv`. Which COPY INTO option should they use?

- A) FILES = ('sales_2025*.csv')
- B) PATTERN = '.*sales_2025.*[.]csv'
- C) WHERE filename LIKE 'sales_2025%'
- D) INCLUDE_PATTERN = 'sales_2025*'

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) PATTERN = '.*sales_2025.*[.]csv'**

**Explanation:** PATTERN accepts a Java-style regular expression to filter files. The regex `'.*sales_2025.*[.]csv'` matches any file containing "sales_2025" with a .csv extension. The FILES option requires explicit file names (no wildcards). There is no WHERE or INCLUDE_PATTERN option for COPY INTO.

**Exam Trap:** PATTERN = regex (for pattern matching). FILES = explicit list (for specific files). They cannot be used together.

</details>

---

### Question 75
A Snowflake account on GCP uses Snowpipe with auto-ingest. What GCP service delivers notifications to Snowpipe?

- A) Cloud Functions
- B) Pub/Sub subscription
- C) Cloud Scheduler
- D) Cloud Storage Triggers

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Pub/Sub subscription**

**Explanation:** On Google Cloud Platform, Snowpipe auto-ingest uses GCS notifications routed through a Pub/Sub subscription that Snowflake manages. When files land in the GCS bucket, a notification is published to the topic, and Snowflake's subscription triggers Snowpipe. AWS uses SQS, Azure uses Event Grid, GCP uses Pub/Sub.

**Exam Trap:** Know the event notification mechanism per cloud: AWS = SQS, Azure = Event Grid, GCP = Pub/Sub.

</details>

---

### Question 76
A data engineer is unloading a 500GB table using COPY INTO @external_stage. The operation produces 31,250 output files (default 16MB each). For downstream processing, they need exactly ONE output file. What change should they make?

- A) Set MAX_FILE_SIZE = 500000000000 (500GB)
- B) Set SINGLE = TRUE — but be aware this eliminates parallelism and will be significantly slower
- C) Set PARTITION BY = NONE
- D) Set FILE_COUNT = 1

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Set SINGLE = TRUE — but be aware this eliminates parallelism and will be significantly slower**

**Explanation:** SINGLE = TRUE forces the unload to produce a single output file. However, this eliminates parallel writing, making the operation much slower for large datasets. For 500GB, this could take significantly longer than the parallel default. There is no FILE_COUNT or PARTITION BY = NONE option.

**Exam Trap:** SINGLE = TRUE = one file but serial (slow). Default = many parallel files (fast). Consider the performance tradeoff for large datasets.

</details>

---

### Question 77
A COPY INTO load command processes 10 CSV files totaling 2GB. The default ON_ERROR (ABORT_STATEMENT) is used. File 8 has a single malformed row. What is the total data loaded?

- A) 7 files worth of data (files 1-7)
- B) ZERO — ABORT_STATEMENT rolls back ALL files, including the 7 successful ones
- C) All data except the single bad row
- D) 8 files (file 8 is partially loaded up to the error row)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) ZERO — ABORT_STATEMENT rolls back ALL files, including the 7 successful ones**

**Explanation:** ABORT_STATEMENT (the default) treats the entire COPY INTO as an atomic transaction. If ANY error occurs in ANY file, the entire operation is rolled back — no data from any file is committed. This is the safest option for data integrity but means one bad row kills the entire batch.

**Exam Trap:** ABORT_STATEMENT = all-or-nothing. Even 1 error in 1 file rolls back all files. For partial loading, use SKIP_FILE or CONTINUE.

</details>

---

### Question 78
A data pipeline loads files via COPY INTO every hour. After 65 days, an identical file from day 1 (same name, same content) reappears in the stage. What happens when COPY INTO runs?

- A) The file is skipped because Snowflake permanently tracks loaded files
- B) The file is LOADED AGAIN because COPY INTO's 64-day metadata has expired — creating potential duplicates
- C) Snowflake detects the duplicate content by checksum and skips it regardless of metadata
- D) The file is quarantined for manual review

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The file is LOADED AGAIN because COPY INTO's 64-day metadata has expired — creating potential duplicates**

**Explanation:** COPY INTO maintains load metadata for exactly 64 days. After 64 days, the tracking record for that file is purged. If the file reappears in the stage, COPY INTO treats it as never-before-seen and loads it again. This can create duplicates. Best practice: remove files from the stage after loading (PURGE = TRUE) or implement idempotent loading.

**Exam Trap:** COPY INTO metadata = 64 days. Snowpipe metadata = 14 days. After expiry, reappearing files are re-loaded.

</details>

---

### Question 79
A team uses Snowpipe Streaming (via the Snowflake Ingest SDK) for sub-second latency loading. Their colleague suggests using traditional Snowpipe (auto-ingest) for the same use case. What is the key difference?

- A) No difference — both achieve sub-second latency
- B) Snowpipe Streaming writes rows directly into Snowflake tables via API (sub-second latency), while traditional Snowpipe loads files from stages (minutes of latency)
- C) Snowpipe Streaming is cheaper but slower
- D) Traditional Snowpipe supports Streaming but requires a file format definition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Snowpipe Streaming writes rows directly into Snowflake tables via API (sub-second latency), while traditional Snowpipe loads files from stages (minutes of latency)**

**Explanation:** Snowpipe Streaming uses the Ingest SDK to write rows directly into Snowflake without staging files — achieving sub-second latency. Traditional Snowpipe requires files to land in a stage, then loads them (typically 1-2 minute latency). They're different products for different latency requirements.

**Exam Trap:** Snowpipe Streaming = no files, sub-second, SDK-based. Traditional Snowpipe = file-based, minute-level latency, event-driven.

</details>

---

### Question 80
A data engineer wants to validate that their file format and column mapping will work BEFORE actually loading 500GB of data. They run COPY INTO with VALIDATION_MODE = 'RETURN_5_ROWS'. The command succeeds and returns 5 sample rows. They then run COPY INTO without VALIDATION_MODE. What should they be aware of?

- A) The 5 validated rows won't be loaded again
- B) VALIDATION_MODE registered the file in load metadata — they may need FORCE = TRUE for the actual load
- C) The actual load will skip the first 5 rows since they were already validated
- D) No concerns — the actual load will process all rows normally

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) VALIDATION_MODE registered the file in load metadata — they may need FORCE = TRUE for the actual load**

**Explanation:** VALIDATION_MODE validates without loading BUT registers the file in the load metadata system. When the engineer runs the actual COPY INTO, the metadata check sees the file as "already processed" and skips it. They need FORCE = TRUE, or they need to use a different file name, or they should purge the metadata by waiting 64 days.

**Exam Trap:** VALIDATION_MODE = no data loaded, but file IS registered in metadata. Use FORCE = TRUE for the real load afterward.

</details>

---

### Question 81
An organization loads semi-structured data (JSON, Parquet) and structured data (CSV) using the same pipeline. For CSV files, COPY INTO with ABORT_STATEMENT default works fine. But for their 2GB JSON files, a single bad record kills the entire load. What approach provides per-record error handling for JSON?

- A) Use ON_ERROR = CONTINUE to skip individual bad JSON records while loading all valid ones
- B) JSON files cannot be loaded with error handling — they must be fixed beforehand
- C) Split the JSON file into 1-record-per-file to make SKIP_FILE work at record level
- D) Use VALIDATE_JSON() as a pre-filter

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Use ON_ERROR = CONTINUE to skip individual bad JSON records while loading all valid ones**

**Explanation:** ON_ERROR = CONTINUE works for JSON loading just as for CSV — it skips individual records (JSON objects) that have parsing errors and loads all valid records. This is the simplest approach for handling occasional bad records in large semi-structured files without blocking the entire load.

**Exam Trap:** ON_ERROR = CONTINUE provides row-level (record-level) error tolerance for ALL file formats, including semi-structured.

</details>

---

### Question 82
A team's COPY INTO statement includes a transformation: `COPY INTO t1 FROM (SELECT $1:id::INT, $1:name::STRING, CURRENT_TIMESTAMP() FROM @stage)`. This adds a load timestamp to each row. Is this valid?

- A) No — COPY INTO does not support SELECT syntax
- B) No — functions like CURRENT_TIMESTAMP() cannot be used in COPY INTO transformations
- C) Yes — COPY INTO supports SELECT with column transformations, casting, reordering, and simple functions
- D) Yes — but only for internal stages, not external stages

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Yes — COPY INTO supports SELECT with column transformations, casting, reordering, and simple functions**

**Explanation:** COPY INTO with a FROM (SELECT ...) clause supports column selection, reordering, casting, omission, and system functions (CURRENT_TIMESTAMP, METADATA$FILENAME, etc.). It does NOT support JOINs, GROUP BY, aggregations, or subqueries. Simple transformations and metadata enrichment are valid.

**Exam Trap:** COPY INTO SELECT supports: casting, reordering, column omission, metadata columns, simple functions. Does NOT support: JOINs, GROUP BY, subqueries.

</details>

---

### Question 83
A data pipeline processes files from an external S3 stage. The data engineer notices that COPY_HISTORY shows a file loaded 3 times on the same day. The FORCE option is NOT set. What could explain this?

- A) The file was modified and re-uploaded with different content but the same name — COPY INTO detects content changes via checksum and reloads
- B) Three different COPY INTO commands ran with different file format options, each treating the file as new
- C) A bug in Snowflake's metadata system
- D) The file was genuinely uploaded, deleted, and re-uploaded 3 times — each re-upload is treated as a new file event

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) The file was modified and re-uploaded with different content but the same name — COPY INTO detects content changes via checksum and reloads**

**Explanation:** COPY INTO tracks files by both name AND content hash (checksum/ETag). If a file is re-uploaded with the same name but different content, the metadata system recognizes it as a new version and loads it again. This is intentional — same name + different content = new data to load. This differs from Snowpipe which tracks by name only.

**Exam Trap:** COPY INTO tracks by name + content hash. Same name + different content = reloaded. Snowpipe tracks by name only (14 days).

</details>

---

### Question 84
A company unloads data with PARTITION BY to_date(event_time) and creates date-organized folders in their external stage. They then want to load ONLY yesterday's partition back into a different table. What approach works?

- A) COPY INTO new_table FROM @stage/date=2025-06-15/ — reference the partition path directly in the stage location
- B) Use PARTITION_FILTER = 'yesterday' in COPY INTO
- C) Query the external stage with a WHERE clause
- D) Use SELECT FROM @stage WHERE $1:event_date = yesterday

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) COPY INTO new_table FROM @stage/date=2025-06-15/ — reference the partition path directly in the stage location**

**Explanation:** PARTITION BY during unload creates a folder structure (e.g., /date=2025-06-15/). To load a specific partition, simply reference the path in the stage URL. COPY INTO can target a subfolder within a stage. There is no PARTITION_FILTER option. This is a common and efficient pattern for incremental processing.

**Exam Trap:** Unloaded partition folders can be directly referenced as stage paths in COPY INTO — no special filtering needed.

</details>

---

### Question 85
A Snowpipe has been processing files for 6 months. A new requirement needs the pipe's COPY INTO statement to include a new transformation (adding a computed column). The engineer runs ALTER PIPE to change the COPY statement. What happens?

- A) The change takes effect immediately for new files
- B) ALTER PIPE cannot modify the COPY statement — the pipe must be dropped and recreated (or CREATE OR REPLACE PIPE)
- C) The pipe pauses automatically and requires manual resume after the change
- D) Only the file format portion of COPY can be altered

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) ALTER PIPE cannot modify the COPY statement — the pipe must be dropped and recreated (or CREATE OR REPLACE PIPE)**

**Explanation:** The COPY INTO definition within a pipe is immutable — it cannot be changed via ALTER PIPE. ALTER PIPE can only modify properties like comments or PIPE_EXECUTION_PAUSED. To change the COPY logic, you must DROP and CREATE the pipe again (or use CREATE OR REPLACE PIPE). Consider pausing first to avoid losing in-flight files.

**Exam Trap:** Pipe COPY statements are immutable. ALTER PIPE = properties only. Changing the COPY = drop/recreate the pipe.

</details>

---

### Question 58
What function can you use to validate previously loaded data and get error details after a COPY INTO operation completes?

- A) VALIDATE()
- B) COPY_HISTORY()
- C) LOAD_ERRORS()
- D) GET_ERRORS()

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** The VALIDATE() function allows you to view all errors encountered during a previous COPY INTO execution. It takes the query ID of the COPY INTO statement and returns the error details. Syntax: SELECT * FROM TABLE(VALIDATE(table_name, JOB_ID => 'query_id')).

</details>

---

### Question 59
A data pipeline loads Parquet files into a Snowflake table. The engineer wants to capture the source file name and row number for each loaded record. Which metadata columns should they use?

- A) METADATA$FILENAME and METADATA$FILE_ROW_NUMBER
- B) $FILE_NAME and $ROW_NUM
- C) SOURCE_FILE() and ROW_NUMBER()
- D) STAGE$FILENAME and STAGE$ROW_NUMBER

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** Snowflake provides metadata columns that can be referenced in COPY INTO SELECT statements: METADATA$FILENAME (source file name), METADATA$FILE_ROW_NUMBER (row number within the file), METADATA$FILE_CONTENT_KEY (checksum), and METADATA$FILE_LAST_MODIFIED. These enable data lineage tracking.

</details>

---

### Question 60
A company's data pipeline requires loading files larger than 5 GB. They are using an internal stage. What should they be aware of?

- A) Files larger than 5 GB cannot be loaded into Snowflake
- B) PUT automatically splits files into chunks for upload; the large file can be loaded, but smaller files (100-250 MB) are recommended for better COPY INTO parallelism
- C) They must use an external stage for files over 5 GB
- D) A special license is required for large file support

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** PUT can handle large files by automatically splitting them into chunks for parallel upload. However, for COPY INTO performance, Snowflake recommends splitting source files into 100-250 MB compressed chunks before staging. Large single files reduce loading parallelism because only CSV files can be split by COPY INTO; semi-structured formats load as whole files.

</details>

---

### Question 61
Your team loads data using COPY INTO with MATCH_BY_COLUMN_NAME = 'CASE_INSENSITIVE'. What does this enable?

- A) Columns in the file are matched to table columns by name rather than position
- B) All column names are converted to uppercase before loading
- C) The file's column order must match the table's column order
- D) Only columns with matching cases are loaded

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** MATCH_BY_COLUMN_NAME = 'CASE_INSENSITIVE' (or 'CASE_SENSITIVE') matches columns in semi-structured data files (Parquet, Avro, ORC) to the target table columns by name rather than ordinal position. This is useful when column ordering differs between files and the table.

</details>

---

### Question 62
A Snowflake account has a pipe defined with AUTO_INGEST = TRUE on Azure. What Azure service is used for event notifications?

- A) Azure Functions
- B) Azure Event Grid
- C) Azure Service Bus
- D) Azure Logic Apps

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**

**Explanation:** On Microsoft Azure, Snowpipe auto-ingest uses Azure Event Grid to deliver blob storage event notifications (blob created events) to Snowflake. This triggers Snowpipe to load newly arrived files from Azure Blob Storage or Azure Data Lake Storage.

</details>

---

### Question 63
A data engineer accidentally loads the wrong file. They want to identify which files were loaded in the last hour. Which approach is best?

- A) SELECT * FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY(TABLE_NAME=>'my_table', START_TIME=>DATEADD(HOUR, -1, CURRENT_TIMESTAMP())))
- B) SHOW LOADS ON my_table
- C) SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.LOAD_HISTORY WHERE LAST_HOUR = TRUE
- D) LIST @my_stage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** The INFORMATION_SCHEMA.COPY_HISTORY() table function returns detailed information about data loaded via COPY INTO (and Snowpipe) including file names, row counts, errors, and timestamps. It accepts time range parameters to filter results.

</details>

---

### Question 64
Which statement about loading data into VARIANT columns is TRUE?

- A) VARIANT columns have a maximum size of 16 MB per value
- B) VARIANT columns can only store JSON data
- C) Individual VARIANT values cannot exceed 16 MB in compressed size
- D) VARIANT columns do not support indexing or querying nested elements

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**

**Explanation:** Individual VARIANT values have a maximum uncompressed size limit of 16 MB. VARIANT columns can store any semi-structured data (JSON, Avro, ORC, Parquet, XML). They support dot notation and bracket notation for querying nested elements and can be used with FLATTEN for denormalization.

</details>

---

### Question 65
A financial services company needs to unload sensitive customer data to a partner's S3 bucket. The data must be encrypted. What encryption does Snowflake provide for unloaded files to external stages?

- A) No encryption is applied by default
- B) Snowflake always applies 256-bit AES encryption with a customer-managed key
- C) Client-side encryption can be configured on the stage using a master key
- D) Encryption is only available for internal stages

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**

**Explanation:** For external stages, you can configure client-side encryption by specifying a MASTER_KEY in the stage's encryption settings. This encrypts files before they leave Snowflake. For internal stages, files are automatically encrypted with 128-bit or 256-bit keys. The ENCRYPTION option on the stage definition controls this behavior.

</details>

---

## Bonus: Advanced Scenario Questions

### Question 1
A data engineer runs COPY INTO with ON_ERROR = 'CONTINUE' to load a 2GB CSV file. After loading, they discover 50,000 rows were skipped but need to identify the specific errors. Which function should they call and what parameter is required?
- A) VALIDATE(table_name, JOB_ID => '<query_id>') using the COPY INTO query ID
- B) COPY_HISTORY() filtered by table name and time range
- C) SYSTEM$GET_LOAD_ERRORS(table_name)
- D) SELECT * FROM INFORMATION_SCHEMA.LOAD_ERRORS

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) VALIDATE(table_name, JOB_ID => '<query_id>') using the COPY INTO query ID**
**Explanation:** The VALIDATE() table function returns detailed error information for a specific COPY INTO execution identified by its query ID. COPY_HISTORY shows summary information but not individual row-level errors.
**Exam Trap:** VALIDATE() requires the JOB_ID of the COPY INTO statement, not the table name alone — many candidates forget to capture the query ID.

</details>

---

### Question 2
A Snowpipe has been running for 15 days. On day 15, an operations team member accidentally re-uploads a file that was originally loaded on day 1 with the same filename but updated content. What happens?
- A) Snowpipe reloads the file because the content hash changed
- B) Snowpipe skips the file because the filename matches its 14-day metadata
- C) Snowpipe loads the file because the 14-day metadata has expired for that file
- D) Snowpipe throws a duplicate file error

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Snowpipe loads the file because the 14-day metadata has expired for that file**
**Explanation:** Snowpipe retains load metadata for only 14 days. After 14 days, it has no record of the original file, so the re-uploaded file is treated as new and loaded again, potentially causing duplicates.
**Exam Trap:** COPY INTO metadata lasts 64 days, but Snowpipe metadata is only 14 days — candidates often confuse these two retention periods.

</details>

---

### Question 3
A team needs to load IoT sensor data arriving every 30 seconds as individual 5KB JSON files into Snowflake with minimal latency. They estimate 2,880 files per day. Which loading strategy minimizes cost while maintaining sub-5-minute latency?
- A) Snowpipe auto-ingest on each file arrival
- B) A scheduled COPY INTO task running every 5 minutes
- C) Snowpipe Streaming API with direct row-level inserts
- D) Batch files into 100MB aggregates using an external ETL tool, then use COPY INTO

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Snowpipe Streaming API with direct row-level inserts**
**Explanation:** Snowpipe Streaming (via the Snowflake Ingest SDK) writes rows directly into Snowflake tables without staging files, avoiding per-file overhead charges. For many tiny files with low-latency requirements, it's more cost-effective than traditional Snowpipe (which charges per-file overhead).
**Exam Trap:** Snowpipe auto-ingest works but the per-file overhead on 2,880 tiny files daily adds up — candidates must distinguish between file-based Snowpipe and Streaming Snowpipe.

</details>

---

### Question 4
A data engineer sets VALIDATION_MODE = 'RETURN_5_ROWS' in their COPY INTO command. The first 5 rows parse successfully. They then remove VALIDATION_MODE and run the same COPY INTO. The load fails on row 5001 with a data type error. Why didn't VALIDATION_MODE catch this?
- A) VALIDATION_MODE only validates the header row
- B) RETURN_n_ROWS only validates the first n rows — errors beyond row 5 were not checked
- C) VALIDATION_MODE checks formatting only, not data types
- D) The file was modified between the validation and load runs

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) RETURN_n_ROWS only validates the first n rows — errors beyond row 5 were not checked**
**Explanation:** RETURN_n_ROWS validates exactly the specified number of rows from the beginning of the file. It does not scan the entire file. For full validation, use RETURN_ERRORS or RETURN_ALL_ERRORS which scans every row.
**Exam Trap:** RETURN_n_ROWS gives false confidence if errors are in later rows — always use RETURN_ALL_ERRORS for production validation.

</details>

---

### Question 5
A company has data in GCS and needs both ad-hoc analyst loading (infrequent, large batches) and real-time pipeline loading from the same bucket. They create one external stage. Which combination is correct?
- A) One external stage used by both COPY INTO (ad-hoc) and a Snowpipe with AUTO_INGEST=TRUE (real-time), with GCS Pub/Sub notifications
- B) Two external stages required — one for COPY INTO and one for Snowpipe
- C) One external stage for COPY INTO, plus Snowpipe REST API calls for real-time
- D) Both A and C are valid approaches

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) Both A and C are valid approaches**
**Explanation:** A single external stage can serve both COPY INTO and Snowpipe. Auto-ingest uses GCS Pub/Sub notifications, while the REST API approach uses programmatic notification. Both can coexist on the same stage with different pipe definitions.
**Exam Trap:** GCS uses Pub/Sub (not SQS or Event Grid) — candidates must know the cloud-specific notification mechanism for each provider.

</details>

---

### Question 6
A COPY INTO command specifies SIZE_LIMIT = 500000000 (500MB). The stage contains files: A (200MB), B (200MB), C (200MB), D (200MB). How many files are loaded?
- A) 2 files (A and B = 400MB, under limit)
- B) 3 files (A + B + C = 600MB, but C is included because it started before threshold)
- C) All 4 files
- D) 3 files (once aggregate exceeds 500MB after including C, loading stops — D is excluded)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) 3 files (once aggregate exceeds 500MB after including C, loading stops — D is excluded)**
**Explanation:** SIZE_LIMIT includes whole files until the cumulative size exceeds the threshold. Files A+B = 400MB (under limit, continue), A+B+C = 600MB (exceeds 500MB limit, stop). File C is included because it was committed before the threshold check. File D is not loaded.
**Exam Trap:** SIZE_LIMIT does not split files — it includes complete files and stops AFTER exceeding the threshold, not before.

</details>

---

### Question 7
A table stage (@%my_table) is being used for loading. The data engineer wants to set a default file format on the table stage. What happens?
- A) ALTER STAGE @%my_table SET FILE_FORMAT = (TYPE=CSV) succeeds
- B) The command fails — table stages do not support file format options in their definition
- C) File format is inherited from the table's schema default
- D) You must specify file format in the CREATE TABLE statement

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The command fails — table stages do not support file format options in their definition**
**Explanation:** Table stages cannot be altered independently — they don't support setting file format options, unlike named stages. You must specify the file format directly in the COPY INTO command or use a named file format object referenced in the COPY INTO.
**Exam Trap:** Table stages (@%) are convenient but limited — no ALTER, no file format defaults, no sharing between tables.

</details>

---

### Question 8
A data engineer configures Snowpipe auto-ingest on AWS S3. They create the pipe, set up the SQS notification, and confirm files are landing in S3. But no data is appearing in the target table. What is the MOST likely cause?
- A) The pipe is in a PAUSED state and needs ALTER PIPE ... SET PIPE_EXECUTION_PAUSED = FALSE
- B) The S3 event notification is pointing to the wrong SQS queue ARN
- C) The IAM role doesn't have permissions on the S3 bucket
- D) All of the above are possible, but B is most commonly missed

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) All of the above are possible, but B is most commonly missed**
**Explanation:** The most common auto-ingest failures are: (1) pipe created in paused state, (2) SQS notification ARN mismatch (must use the queue ARN from SHOW PIPES, not a custom queue), (3) IAM permission issues. The SQS ARN must match exactly what Snowflake provides via SYSTEM$PIPE_STATUS or SHOW PIPES.
**Exam Trap:** The SQS queue is Snowflake-managed — you configure S3 to notify Snowflake's queue, not your own.

</details>

---

### Question 9
A COPY INTO uses a SELECT clause to transform data during load: `COPY INTO target FROM (SELECT $1, $2::DATE, UPPER($3) FROM @stage)`. The engineer also wants to filter rows where $4 = 'ACTIVE'. Is this possible?
- A) No — COPY INTO transformations only support column selection and casting, not WHERE filters
- B) Yes — add WHERE $4 = 'ACTIVE' to the subquery
- C) No — any filtering must be done post-load
- D) Yes, but only with ON_ERROR = 'CONTINUE'

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes — add WHERE $4 = 'ACTIVE' to the subquery**
**Explanation:** COPY INTO with a SELECT subquery supports column reordering, casting, functions (like UPPER), and WHERE clause filtering. However, it does NOT support JOINs, GROUP BY, ORDER BY, LIMIT, or multiple tables in the FROM clause.
**Exam Trap:** COPY INTO transformations are more capable than many assume — column expressions, CASE, CAST, and WHERE all work, but no JOINs or aggregations.

</details>

---

### Question 10
A data pipeline loads CSV files where some fields contain the delimiter character (comma) within quoted strings. The load fails with "too many columns" errors. Which file format option resolves this?
- A) FIELD_OPTIONALLY_ENCLOSED_BY = '"'
- B) ESCAPE_UNENCLOSED_FIELD = '\\'
- C) FIELD_DELIMITER = '|' (change delimiter)
- D) TRIM_SPACE = TRUE

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) FIELD_OPTIONALLY_ENCLOSED_BY = '"'**
**Explanation:** Setting FIELD_OPTIONALLY_ENCLOSED_BY = '"' tells Snowflake that field values may be enclosed in double quotes, and commas within quotes should not be treated as delimiters. This is the standard approach for RFC-compliant CSV parsing.
**Exam Trap:** The default is FIELD_OPTIONALLY_ENCLOSED_BY = NONE — Snowflake does NOT assume quote-enclosed fields by default, unlike many other tools.

</details>

---

### Question 11
A user stage (@~) contains files from multiple projects. A data engineer wants to organize them into folders. Can they create subdirectories in a user stage?
- A) Yes — use PUT with path prefixes like @~/project_a/file.csv
- B) No — user stages are flat and do not support directories
- C) Yes, but only via the Snowflake web UI
- D) Only named stages support directories

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Yes — use PUT with path prefixes like @~/project_a/file.csv**
**Explanation:** All internal stages (user, table, named) support path prefixes that simulate directory structures. You can PUT files with paths like @~/folder/subfolder/file.csv. These aren't true filesystem directories but virtual path prefixes.
**Exam Trap:** The path is part of the filename — LIST @~ shows the full path including prefixes; you reference them in COPY INTO with the full path.

</details>

---

### Question 12
A company loads the same set of files using COPY INTO every day as a safety measure. After 64 days, they notice duplicate data appearing. Why?
- A) The Time Travel retention expired, resetting load metadata
- B) COPY INTO load metadata expired after 64 days, so files are treated as new
- C) The FORCE = TRUE option was accidentally set
- D) The files were modified, changing their checksum

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) COPY INTO load metadata expired after 64 days, so files are treated as new**
**Explanation:** COPY INTO tracks loaded files for 64 days via load metadata. After 64 days, if the same files are still in the stage, COPY INTO has no record of them and loads them again. Solution: either PURGE files after loading or design pipelines that don't re-present files after 64 days.
**Exam Trap:** 64-day metadata expiry is a common cause of production duplicates — always PURGE or move files out of the stage after loading.

</details>

---

### Question 13
A data engineer needs to load a 50GB Parquet file from S3 into a structured Snowflake table (not VARIANT). They want each Parquet column mapped to a corresponding table column. What is the correct approach?
- A) COPY INTO target FROM @stage MATCH_BY_COLUMN_NAME = 'CASE_INSENSITIVE'
- B) COPY INTO target FROM (SELECT $1:col1::INT, $1:col2::STRING FROM @stage)
- C) Both A and B are valid approaches
- D) Parquet files must always load into a single VARIANT column first

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Both A and B are valid approaches**
**Explanation:** MATCH_BY_COLUMN_NAME automatically maps Parquet columns to table columns by name. Alternatively, a SELECT with explicit column extraction from $1 (the VARIANT representation) with casting achieves the same result. The first approach is simpler; the second gives more control over transformations.
**Exam Trap:** D is false — Parquet does NOT require VARIANT; direct column mapping has been supported since the MATCH_BY_COLUMN_NAME feature.

</details>

---

### Question 14
A Snowpipe is configured with AUTO_INGEST=TRUE on an Azure external stage. The notification uses Event Grid. A new file lands in Blob Storage but is not loaded after 10 minutes. SYSTEM$PIPE_STATUS shows executionState = 'RUNNING' and pendingFileCount = 0. What does this indicate?
- A) The pipe is working but the file is too large to process
- B) The Event Grid notification never reached Snowflake — the file wasn't detected
- C) The file was loaded but into the wrong table
- D) The pipe needs to be recreated

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The Event Grid notification never reached Snowflake — the file wasn't detected**
**Explanation:** pendingFileCount = 0 with executionState = 'RUNNING' means the pipe is active but has no files queued. The event notification likely failed — check Event Grid subscription configuration, storage account event routing, and that the notification endpoint matches Snowflake's integration.
**Exam Trap:** SYSTEM$PIPE_STATUS is essential for debugging — pendingFileCount = 0 means notification failure, not pipe failure.

</details>

---

### Question 15
A COPY INTO command specifies PURGE = TRUE. The load encounters errors on 3 out of 10 files with ON_ERROR = 'SKIP_FILE'. What happens to the files?
- A) All 10 files are purged (deleted from stage)
- B) Only the 7 successfully loaded files are purged; the 3 error files remain
- C) No files are purged because some files had errors
- D) All files are purged and errors are logged to an error table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Only the 7 successfully loaded files are purged; the 3 error files remain**
**Explanation:** PURGE = TRUE only removes files that were successfully loaded. Files that were skipped due to errors remain in the stage for investigation and retry. This behavior ensures you don't lose data that failed to load.
**Exam Trap:** PURGE is per-file, not all-or-nothing — only successful files are removed.

</details>

---

### Question 16
A team uses PUT to upload a 1GB file from their local machine. The upload fails repeatedly at 80% completion. What PUT option should they adjust?
- A) PARALLEL = 1 (reduce parallelism to avoid timeout)
- B) AUTO_COMPRESS = FALSE (skip compression to reduce processing time)
- C) OVERWRITE = TRUE (ensure partial uploads are replaced)
- D) Reduce the file size to under 250MB before uploading

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) Reduce the file size to under 250MB before uploading**
**Explanation:** PUT uploads large files by splitting them into chunks, but network instability can still cause failures on very large files. Splitting the source file into smaller pieces (100-250MB recommended) improves upload reliability and subsequent COPY INTO parallelism.
**Exam Trap:** PUT and GET only work via SnowSQL, JDBC, or ODBC — they are unavailable through the web UI or REST API.

</details>

---

### Question 17
A company loads JSON data with STRIP_OUTER_ARRAY = TRUE. Their file contains: `[{"id":1},{"id":2},{"id":3}]`. After loading, they find only 1 row in the table. What went wrong?
- A) The file was loaded without STRIP_OUTER_ARRAY in a previous run, and metadata is preventing reload
- B) The JSON is invalid
- C) They loaded into a VARIANT column and the entire array became one row — they forgot the STRIP_OUTER_ARRAY option in the actual load (only had it in file format definition but not the stage)
- D) The file has a BOM character preventing proper parsing

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) They loaded into a VARIANT column and the entire array became one row — they forgot the STRIP_OUTER_ARRAY option in the actual load (only had it in file format definition but not the stage)**
**Explanation:** Without STRIP_OUTER_ARRAY = TRUE, the entire JSON array [{"id":1},{"id":2},{"id":3}] is loaded as a single VARIANT value (one row). The option must be active in the file format being used by the COPY INTO command.
**Exam Trap:** Always verify which file format object is actually being applied — stage default, named format, or inline options can override each other.

</details>

---

### Question 18
An external stage is defined with a storage integration pointing to s3://bucket/data/. A COPY INTO references this stage with path @ext_stage/2024/january/. What is the full S3 path being accessed?
- A) s3://bucket/data/2024/january/
- B) s3://bucket/2024/january/
- C) s3://bucket/data/ext_stage/2024/january/
- D) s3://2024/january/

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) s3://bucket/data/2024/january/**
**Explanation:** The stage URL provides the base path (s3://bucket/data/), and the path specified after the stage name in COPY INTO is appended to it. So @ext_stage/2024/january/ resolves to s3://bucket/data/2024/january/.
**Exam Trap:** The path in COPY INTO is relative to the stage's URL — it doesn't replace it.

</details>

---

### Question 19
A data pipeline uses COPY INTO with FORCE = TRUE to reload corrected files. After the reload, they discover duplicate records because the original (incorrect) data was not removed first. What is the correct procedure?
- A) Use COPY INTO with FORCE = TRUE and TRUNCATECOLUMNS = TRUE
- B) TRUNCATE the target table before running COPY INTO with FORCE = TRUE
- C) DELETE rows from the target table that match the file's timestamp, then COPY INTO with FORCE = TRUE
- D) Both B and C are valid, depending on whether a full or partial reload is needed

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) Both B and C are valid, depending on whether a full or partial reload is needed**
**Explanation:** FORCE = TRUE bypasses duplicate detection but doesn't remove existing data. For a full reload, TRUNCATE first. For a partial correction, DELETE the affected rows first. FORCE alone always adds rows — it never replaces.
**Exam Trap:** FORCE = TRUE is not an "overwrite" — it's "load regardless of metadata." It always INSERTs, never UPDATEs or DELETEs.

</details>

---

### Question 20
A team runs COPY INTO from an external stage referencing 1000 files. They use PATTERN = '.*2024.*[.]parquet' to filter. Only 50 files match the pattern. How does this affect performance compared to using the FILES option with the 50 filenames explicitly listed?
- A) PATTERN is faster because it uses server-side regex filtering
- B) FILES is faster because PATTERN requires listing all 1000 files first to apply the regex
- C) They perform identically
- D) FILES is faster for small numbers of files; PATTERN is faster for large matches

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) FILES is faster because PATTERN requires listing all 1000 files first to apply the regex**
**Explanation:** PATTERN triggers a LIST operation on the stage to identify matching files, which adds overhead proportional to the total number of files. FILES with explicit names skips the listing step entirely. For stages with many files but few matches, FILES is more efficient.
**Exam Trap:** PATTERN convenience comes at a cost — on stages with millions of files, the LIST operation itself can be slow.

</details>

---
