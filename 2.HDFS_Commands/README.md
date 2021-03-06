Hadoop Commands
===============
```
-ls <path>
Lists the contents of the directory specified by path, showing the names, permissions, owner, size and modification date for each entry.
-lsr <path>
Behaves like -ls, but recursively displays entries in all subdirectories of path.
-du <path>
Shows disk usage, in bytes, for all the files which match path; filenames are reported with the full HDFS protocol prefix.
-dus <path>
Like -du, but prints a summary of disk usage of all files/directories in the path.
-mv <src><dest>
Moves the file or directory indicated by src to dest, within HDFS.
-cp <src> <dest>
Copies the file or directory identified by src to dest, within HDFS.
-rm <path>
Removes the file or empty directory identified by path.
-rmr <path>
Removes the file or directory identified by path. Recursively deletes any child entries (i.e., files or subdirectories of path).
-put <localSrc> <dest>
Copies the file or directory from the local file system identified by localSrc to dest within the DFS.
-copyFromLocal <localSrc> <dest>
Identical to -put
-moveFromLocal <localSrc> <dest>
Copies the file or directory from the local file system identified by localSrc to dest within HDFS, and then deletes the local copy on success.
-get [-crc] <src> <localDest>
Copies the file or directory in HDFS identified by src to the local file system path identified by localDest.
-getmerge <src> <localDest>
Retrieves all files that match the path src in HDFS, and copies them to a single, merged file in the local file system identified by localDest.
-cat <filen-ame>
Displays the contents of filename on stdout.
-copyToLocal <src> <localDest>
Identical to -get
-moveToLocal <src> <localDest>
Works like -get, but deletes the HDFS copy on success.
-mkdir <path>
Creates a directory named path in HDFS.
Creates any parent directories in path that are missing (e.g., mkdir -p in Linux).
-setrep [-R] [-w] rep <path>
Sets the target replication factor for files identified by path to rep. (The actual replication factor will move toward the target over time)
-touchz <path>
Creates a file at path containing the current time as a timestamp. Fails if a file already exists at path, unless the file is already size 0.
-test -[ezd] <path>
Returns 1 if path exists; has zero length; or is a directory or 0 otherwise.
-stat [format] <path>
Prints information about path. Format is a string which accepts file size in blocks (%b), filename (%n), block size (%o), replication (%r), and modification date (%y, %Y).
-tail [-f] <file2name>
Shows the last 1KB of file on stdout.
-chmod [-R] mode,mode,... <path>...
Changes the file permissions associated with one or more objects identified by path.... Performs changes recursively with R. mode is a 3-digit octal mode, or {augo}+/-{rwxX}. Assumes if no scope is specified and does not apply an umask.
-chown [-R] [owner][:[group]] <path>...
Sets the owning user and/or group for files or directories identified by path.... Sets owner recursively if -R is specified.
-chgrp [-R] group <path>...
Sets the owning group for files or directories identified by path.... Sets group recursively if -R is specified.
-help <cmd-name>
Returns usage information for one of the commands listed above. You must omit the leading '-' character in cmd.

appendToFile
Usage: hdfs dfs -appendToFile <localsrc> ... <dst>

Append single src, or multiple srcs from local file system to the destination file system. Also reads input from stdin and appends to destination file system.

hdfs dfs -appendToFile localfile /user/hadoop/hadoopfile
hdfs dfs -appendToFile localfile1 localfile2 /user/hadoop/hadoopfile
hdfs dfs -appendToFile localfile hdfs://nn.example.com/hadoop/hadoopfile
hdfs dfs -appendToFile - hdfs://nn.example.com/hadoop/hadoopfile Reads the input from stdin.

```

Copy from local file to hdfs
============================
hdfs dfs -put /vagrant/data/star2002-full.csv /user/vscript/

Command to see blocks and replication
=====================================

  * hadoop fsck / -files -blocks -locations
  * hadoop fsck /user/vscript/star2002-full.csv -files -blocks -locations
  * hdfs getconf -confKey dfs.blocksize

** change the default block size in hdfs-site.xml **
$HADOOP_HOME/conf/hdfs-site.xml
<property>
  <name>dfs.block.size</name>
  <value>134217728</value>
  128*1024*1024
</property...

Command to see Hadoop jobs running 
==================================
```
ps -ef | grep hadoop | grep -P  'namenode|datanode|tasktracker|jobtracker'
jps
```
