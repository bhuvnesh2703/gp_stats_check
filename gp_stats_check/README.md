#gp_stats_check

##Brief: 
Python utility to identify tables with outdated statistics involved in a query. It accepts an input file holding query of the form: EXPLAIN VERBOSE . It will execute the input file and scan explain verbose plan of the query to identify the tables. This utility will compare the recort count of the tables with the reltuple information available in pg_class table. If the difference between record count and reltuples is within a defined percentage, stats are considered to be up-to-date else are marked as outdated. It will also capture explain plan for the query in the log file in current working directory.

    Usage: gp_stats_check [options] Example: ./gp_stats_check -f query1.sql -p port -d database

    Options: 
    -h, --help                                  show this help message and exit 
    -f QUERYFILE, --queryfile=QUERYFILE         Filename holding SQL 
    -?, --usage 
    -v, --version 

    Database specific optional arguments: 
    -p PORT, --port=PORT                        Port of database to connect [default: 5432] 
    -d DATABASE, --database=DATABASE            Name of the database to connect [default: template1]


##Example output:
Note: With Greenplum new optimizer ORCA, stats on top level partitions are important and are used for evaluating best plan. Statistics on child partitions are not required. Yet to enhance gp_stats_check with it, until that, please take it into consideration while verifying outputs.

    [INFO]:- Execution started. Please refer to log file name: gp_stats_check_20150411204037.log for details in current working directory.
    [INFO]:- Input query will be executed using the legacy planner.
    [INFO]:- No of tables scanned during user query execution: 8.
    [INFO]:- Stats check in progress, please wait for completion, as it may take time.
    Tables scanned: 8
                                                               Stat check summary
    ----------------------------------------------------------------------------------------------------------------------------------------
    | Table Name                         | Record Count     | Estimated Count     | Variation     | Comments - based on variation %        |
    ----------------------------------------------------------------------------------------------------------------------------------------
    | foo.address                        | 1000             | 1000                | 0             | Stats seems ok                         |
    | foo.emp_salary                     | 199              | 199                 | 0             | Stats seems ok                         |
    | foo.emp                            | 1500             | 500                 | 1000          | Stats are outdated, please ANALYZE     |
    | foo.geography_1_prt_xxypacific     | 50               | 0                   | 50            | Stats are outdated, please ANALYZE     |
    | foo.geography_1_prt_xxxyindian     | 50               | 0                   | 50            | Stats are outdated, please ANALYZE     |
    | foo.geography_1_prt_other          | 0                | 0                   | 0             | Stats seems ok                         |
    ----------------------------------------------------------------------------------------------------------------------------------------
    [INFO]:- Differences between record count and estimated count higher than 5% variation suggests outdated stats
    [INFO]:- To analyze tables with outdated stats, please run: psql -d warehouse -f gp_stats_check_20150411204037.sql
    [INFO]:- Stats check for below tables is not required, as they are either view or top level partition
    -----------------------------------------------
    | Table Name        | Table Type              |
    -----------------------------------------------
    | foo.salary        | View                    |
    | foo.geography     | Top level partition     |
    -----------------------------------------------
    [INFO]:- Execution finished successfully !!
