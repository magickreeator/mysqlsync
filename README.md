mysqlsync
=========

Tool for fast incremental syncing of MySQL repositories

mysqlsync is a program for Unix-like operating systems to incrementally sync
structure and data between from one MySQL database to another. mysqlsync was
written with raw performance and simplicity in mind. The sync is only one-way,
from DB1 to DB2. DB1 is never touched.

The mysql client programs (mysql) must be installed.

mysqlsync has only been tested with the GNU toolchain, i.e. you will be able
to run it fine on linux etc., but it will probably not work on BSD, OS X, etc.

NB. mysql will sometimes store values in tables that do not conform to the
table's schema and hence cannot be simply copied over with its standard tools.
If you get errors due to this, please fix the data in your source database
first.


Usage
=====

mysqlsync [OPTIONS] USER1[/PASS1]@HOST1:DB1 USER2[/PASS2]@HOST2:DB2

OPTIONS:
    --struct-check-only
        Only check for different structures, do not inspect or change table data
        at all. Useful for minor structure differences you want to handle
        manually rather than allowing the default behaviour (remove and re-create
        remote table). This overrides options like --drop-tables or --delete.

    -m, --mtime-col TABLE:COL[,TABLE2:COL2 ...]
        Specify an indexed column, if it exists, designating each row's last-modified
        time. This will be used to partition the data and avoid synchronising
        partitions with matching counts. You may specify this option multiple times or
        specify multiple table:column pairs separated by commas.

    --drop-tables
        Drop tables in DB2 that are not in DB1

    --delete
        Delete rows in tables in DB2 that are not in DB1.

    --cache-checksums
        Cache table checksums and used cached checksums when available. This
        is useful if mysqlsync exits before fully syncing the data. The caches
        aer kept in HOME.

    -s, --skip-table TABLE
        Skip TABLE from sync. To skip multiple tables, specify the option
        multiple times.

    --skip-pattern PATTERN
        Skip tables matching PATTERN from sync. To skip multiple tables,
        give the option multiple times.
    
    -o, --only TABLE
        Only sync TABLE. To inclue multiple tables, specify the option multiple
        times.

    -t, --tmpdir TMPDIR
        Use TMPDIR as a staging directory instead of $TMPDIR.

    --net-buffer-length BUFLEN
        Net buffer length. Default 16777216. If you are seeing errors, try
        setting a lower value or checking your server's configuration.

    --max-allowed-packet MAXPKT
        Maximum allowed packet size. Default 1073741824. If you are seeing
        errors, try setting a lower value or checking your server's configuration.

    --threads NUM-THREADS
        Concurrency of queries against each database. Default value is 4.

    --truncate-table TABLE
        Truncate TABLE in the remote database. Useful for cache tables that you
        are not transferring.

    --truncate-pattern PATTERN
        Truncate tables matching PATTERN in the remote database. See documentation
        for --truncate-table.
    
    -V, --version
        Print version information

