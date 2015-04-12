gp_stats_check

Brief: Python utility to identify tables with outdated statistics involved in a query. It accepts an input file holding query of the form: EXPLAIN VERBOSE . It will execute the input file and scan explain verbose plan of the query to identify the tables. This utility will compare the recort count of the tables with the reltuple information available in pg_class table. If the difference between record count and reltuples is within a defined percentage, stats are considered to be up-to-date else are marked as outdated. It will also capture explain plan for the query in the log file in current working directory.

    Usage: gp_stats_check [options] ./gp_stats_check -f query1.sql -p database port

    Options: 
    -h, --help					show this help message and exit 
    -f QUERYFILE, --queryfile=QUERYFILE		Filename holding SQL 
    -?, --usage 
    -v, --version 

    Database specific optional arguments: 
    -p PORT, --port=PORT				Port of database to connect [default: 5432] 
    -d DATABASE, --database=DATABASE		Name of the database to connect [default: template1]
