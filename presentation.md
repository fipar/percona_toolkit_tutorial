Percona Toolkit Tutorial
===

Percona Live New York 2012

!

Introduction
===

!

About us
===
- Marcos Albe, Support Engineer @ Percona
- Fernando Ipar, Senior Consultant @ Percona

!

About this tutorial
===
- We'll review all the tools in the kit
- Won't be in depth
  - Focused on most frequently used tools
- As hands on as time permits
- More specific examples in our 'recipes' talk tomorrow

!


Testing VMs
===

	https://github.com/fipar/vagrant_pt_demos

!

The toolkit
===

!

<table align=center>
	<tr>
		<td rowspan=3>Utility</td>
		<td>pt-find</td>
	</tr>
	<tr>
		<td class=highlight>pt-kill</td>
	</tr>
	<tr>
		<td>pt-tcp-model</td>
	</tr>
	<tr>
		<td colspan=2><hr/></td>
	</tr>
	<tr>
		<td rowspan=4>Unix-related</td>
		<td>pt-fifo-split</td>
	</tr>
	<tr>
		<td>pt-ioprofile</td>
	</tr>
	<tr>
		<td>pt-pmp</td>
	</tr>
	<tr>
		<td>pt-align</td>
	</tr>
	<tr>
		<td colspan=2><hr/></td>
	</tr>	
	<tr>
		<td rowspan=3>Do things</td>
		<td class=highlight>pt-archiver</td>
	</tr>
	<tr>
		<td>pt-log-player</td>
	</tr>
	<tr>
		<td class=highlight>pt-online-schema-change</td>
	</tr>
	<tr>
		<td colspan=2><hr/></td>
	</tr>	
	<tr>
		<td rowspan=6>Learn things</td>
		<td class=highlight>pt-summary</td>
	</tr>
	<tr>
		<td class=highlight>pt-mysql-summary</td>
	</tr>
	<tr>
		<td>pt-config-diff</td>
	</tr>
	<tr>
		<td>pt-variable-advisor</td>
	</tr>
	<tr>
		<td>pt-duplicate-key-checker</td>
	</tr>
	<tr>
		<td>pt-mext</td>
	</tr>	
</table>

!

<table align=center>
	<tr>
		<td rowspan=4>Record things</td>
		<td>pt-deadlock-logger</td>
	</tr>
	<tr>
		<td>pt-fk-error-logger</td>
	</tr>
	<tr>
		<td>pt-show-grants</td>
	</tr>
	<tr>
		<td>pt-diskstats</td>
	</tr>
	<tr>
		<td colspan=2><hr/></td>
	</tr>	
	<tr>
		<td rowspan=6>Replication related</td>
		<td class=highlight>pt-heartbeat</td>
	</tr>
	<tr>
		<td>pt-slave-delay</td>
	</tr>
	<tr>
		<td>pt-slave-find</td>
	</tr>
	<tr>
		<td class=highlight>pt-slave-restart</td>
	</tr>
	<tr>
		<td class=highlight>pt-table-checksum</td>
	</tr>
	<tr>
		<td class=highlight>pt-table-sync</td>
	</tr>
	<tr>
		<td colspan=2><hr/></td>
	</tr>	
	<tr>
		<td rowspan=8>Query related</td>
		<td>pt-query-advisor</td>
	</tr>
	<tr>
		<td>pt-index-usage</td>
	</tr>
	<tr>
		<td class=highlight>pt-query-digest</td>
	</tr>
	<tr>
		<td>pt-visual-explain</td>
	</tr>
	<tr>
		<td class=highlight>pt-upgrade</td>
	</tr>
	<tr>
		<td>pt-trend</td>
	</tr>
	<tr>
		<td>pt-fingerprint</td>
	</tr>
	<tr>
		<td>pt-table-usage</td>
	</tr>
</table>

!

<table align=center>
	<tr>
		<td rowspan=2>Troubleshooting</td>
		<td class=highlight>pt-stalk</td>
	</tr>
	<tr>
		<td class=highlight>pt-sift</td>
	</tr>
</table>

!

Utility
===

!

pt-find
===

- find tables and take actions
- Like the unix 'find' command
- Works from SHOW TABLES and if needed SHOW TABLE STATUS

!


	pt-find --mtime +30 --engine Innodb \
	 --exec "ALTER TABLE %D.%N ENGINE=MyISAM"


!

pt-kill
===

- Watch SHOW PROCESSLIST
- Select processes, or groups of processes
- Apply filters
- Take a specified action

!

	pt-kill --busy-time 60 --kill --interval 30
  
!

pt-tcp-model
===

- Analyses tcpdump data
- Desgined to help scalability and performance analysis

!

Unix-related
===

!

pt-fifo-split
===

- Reads lines from a file
- Prints them to a FIFO
- EOF every N lines
- Useful for loading large files into MySQL

!

pt-ioprofile
===

- <span class="warning">Dangerous tool!</span>
- Attaches strace to a process
- Captures I/O-related system calls
- Generates a profile of I/O activity

!

pt-pmp
===

- <span class="warning">Dangerous tool!</span>
- Attaches gdb to a process
- Prints out stack traces
- Aggregates them
- Useful for bottleneck analysis
- Inspired by http://poormansprofiler.org/
  
!

pt-align
===

- Aligns sloppy output
- For example: vmstat, iostat

!

Do things
===

!

pt-archiver
===

- Retrieve rows from a table
- Write them to another table, or file
- Delete them from the first table
- Many safety checks and features
- Extensible via plugins
  
!

pt-log-player
===

- For load simulation
- Splits a query log into several
- Replays them multi-threadedly
- Probably obsoleted by Percona Playback
- pt-query-digest --execute is another alternative

!

pt-online-schema-change
===

- Perform ALTER without locking
- Creates a new table
- Copies rows to it
- Uses triggers to track updates to original table
- Swaps original/new table
- Many safety/sanity features


!

	pt-online-schema-change --execute --alter "add column c int" \ 
	  D=sbtest,t=sbtest1,h=localhost,u=percona

!

Learn things
===

!

pt-summary
===

- Report on system config
- Goals: diff-able, email-able
- Easily extensible

!

pt-mysql-summary
===

- Like pt-summary, but for MySQL

!

pt-config-diff
===

- Compare two servers
- Compare a server to its my.cnf

!

pt-variable-advisor
===

- “Best practices” analysis of configuration
- Works from SHOW VARIABLES

!

pt-duplicate-key-checker
===

- Analyze indexes and foreign keys
- Find duplicate/redundant
- Print SQL that can remove them

!

pt-mext
===

- Format SHOW STATUS samples
- Make it easier to see what's changing

!

Record things
===

!

pt-deadlock-logger
===

- Keep track of how often deadlocks happen
- Analyze which tables and indexes are involved

!

pt-fk-error-logger
===

- Ditto, but for foreign keys instead

!

pt-show-grants
===

- Print GRANT statements for all users

!

pt-diskstats
===

- A replacement for iostat -dx

!

Replication related
===

!

pt-heartbeat
===

- Measure replication delay accurately
- Place “quasi-global transaction IDs” into binlogs
- Ease recovery on large and complex topologies

!

pt-slave-delay
===

- Intentionally delay replication
- Works by starting and stopping slave SQL thread

!

pt-slave-find
===

- Automatically discover all servers in a replication relationship
- Print a summary of their status and configuration

!

pt-slave-restart
===

- Restart replication when it fails
- <span class="warning">Please use with care!</span>
  - Skipping errors is not good practice
  - Useful in dire circumstances
	
!

pt-table-checksum
===

- Determine whether replicas have logically the same data as master
- Come to our presentation tomorrow
  
  
!

pt-table-sync
===

- Repair (online!) an out-of-sync replica
- <span class="warning">Use with care! It does change data</span>
- Also possible to do multi-server sync with confict detection, etc

!

Query related
===

!

pt-query-advisor
===

- Analyze a query and point out problems with the SQL
- Analyze an entire log file
- See also tools.percona.com GUI interface

!

pt-index-usage
===

- Make a catalog of tables and indexes
- Analyze a log of queries
- Report on indexes that are unused, queries that have several execution plans, etc

!

pt-query-digest
===

- Retrieve queries from some source
- Pass them through a “pipeline” of filters/transforms
- Aggregate them and generate a report
- Do lots of other nifty/crazy things
  
!

pt-visual-explain
===

- Analyze EXPLAIN and generate a tree-formatted view
- “Reverse engineer” the execution plan
  
!

pt-upgrade
===

- Execute a log of queries on two servers and compare
- Highlight resultset changes, execution time changes, errors, etc

!

pt-trend
===

- Obsolete tool, likely to be removed
- Slices a query log and reports aggregate stats from segments of it
  
!

pt-fingerprint
===

- Converts queries into fingerprints
- Useful to work on lists of queries using multiple tools

!

pt-table-usage
===

- Analyze how queries use tables

!

Troubleshooting
===

!

pt-stalk
===

- Essential tool for diagnosing things
- Many customers run it 24x7 to catch unforeseen problems
- Watches the server and collects information when there is a
    problem
	
!

	pt-stalk --function processlist --variable State \
	  --match statistics --threshold 10
	  
!

pt-sift
===

- Important for helping analyze pt-stalk's collected data
- Not complete; just a helper script!
- Useful for analyzing a lot of collected samples
- See https://github.com/box/RainGauge too!
  
!

Thanks!
===

- marcos.albe/at/percona.com
- fernando.ipar/at/percona.com

!


