wordcount
=========

Hadoop MapReduce word counting with Java

Run with:

    hadoop jar wordcount.jar com.vectorsoft.WordCount "input_folder" "output_folder"

"input_folder" and "output_folder" are folders on HDFS.


Sample
========

# place the jar in Edgenode/Hadoop Node.
CloudxLab: using winscp place the jar in your home directory

```
[lhariharan142303@cxln4 ~]$ ll wordcount
-rwxrwxrwx 1 lhariharan142303 lhariharan142303   61 Aug 11 18:22 runwc.sh
-rw-rw-r-- 1 lhariharan142303 lhariharan142303 6310 Aug 11 18:14 wordcount.jar
-rw-rw-r-- 1 lhariharan142303 lhariharan142303  798 Aug 11 18:18 wordcount.txt
```

# Place the data in HDFS
create directories
==================
create input directory in hdfs. output directory will be created by the program.
```
hdfs dfs -mkdir /user/lhariharan142303/wordcount
hdfs dfs -mkdir /user/lhariharan142303/wordcount/input
```


Place the file in HDFS
====================
Refer the wordcount.txt file in the repository for this demo.
I suggest to create your own file and run the wordcount with that file.
```
hdfs dfs -put wordcount.txt /user/lhariharan142303/wordcount/input/
```

Run the program
===============
Run the shell script or hadoop command to execute the wordcount. please refer below my run. In your case provide your input and output path.
```
cd wordcount
./runwc.sh /user/lhariharan142303/wordcount/input/ /user/lhariharan142303/wordcount/output/
```

Output Log Analysis
===================
**Things to Note: **
   * The url to track the job: http://cxln2.c.thelab-240901.internal:8088/proxy/application_1594743233823_6430/
   * Running job: job_1594743233823_6430
   * Job job_1594743233823_6430 completed successfully
   * File System Counters
   * Job Counters
   * Map-Reduce Framework


```
[lhariharan142303@cxln4 ~]$ [lhariharan142303@cxln4 ~]$ ./runwc.sh /user/lhariharan142303/wordcount/input/ /user/lhariharan142303/wordcount/output/
20/08/11 18:22:40 INFO client.RMProxy: Connecting to ResourceManager at cxln2.c.thelab-240901.internal/10.142.1.2:8050
20/08/11 18:22:40 INFO client.AHSProxy: Connecting to Application History server at cxln2.c.thelab-240901.internal/10.142.1.2:10200
20/08/11 18:22:41 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
20/08/11 18:22:42 INFO input.FileInputFormat: Total input paths to process : 1
20/08/11 18:22:42 INFO mapreduce.JobSubmitter: number of splits:1
20/08/11 18:22:42 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1594743233823_6430
20/08/11 18:22:42 INFO impl.YarnClientImpl: Submitted application application_1594743233823_6430
20/08/11 18:22:43 INFO mapreduce.Job: The url to track the job: http://cxln2.c.thelab-240901.internal:8088/proxy/application_1594743233823_6430/
20/08/11 18:22:43 INFO mapreduce.Job: Running job: job_1594743233823_6430
20/08/11 18:22:53 INFO mapreduce.Job: Job job_1594743233823_6430 running in uber mode : false
20/08/11 18:22:53 INFO mapreduce.Job:  map 0% reduce 0%
20/08/11 18:23:00 INFO mapreduce.Job:  map 100% reduce 0%
20/08/11 18:23:05 INFO mapreduce.Job:  map 100% reduce 100%
20/08/11 18:23:05 INFO mapreduce.Job: Job job_1594743233823_6430 completed successfully
20/08/11 18:23:05 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=2256
                FILE: Number of bytes written=305793
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=957
                HDFS: Number of bytes written=7
                HDFS: Number of read operations=6
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters
                Launched map tasks=1
                Launched reduce tasks=1
                Data-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=53676
                Total time spent by all reduces in occupied slots (ms)=49568
                Total time spent by all map tasks (ms)=4473
                Total time spent by all reduce tasks (ms)=3098
                Total vcore-milliseconds taken by all map tasks=4473
                Total vcore-milliseconds taken by all reduce tasks=3098
                Total megabyte-milliseconds taken by all map tasks=6870528
                Total megabyte-milliseconds taken by all reduce tasks=6344704
        Map-Reduce Framework
                Map input records=50
                Map output records=250
                Map output bytes=1750
                Map output materialized bytes=2256
                Input split bytes=159
                Combine input records=0
                Combine output records=0
                Reduce input groups=1
                Reduce shuffle bytes=2256
                Reduce input records=250
                Reduce output records=1
                Spilled Records=500
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=116
                CPU time spent (ms)=3210
                Physical memory (bytes) snapshot=1448214528
                Virtual memory (bytes) snapshot=11952807936
                Total committed heap usage (bytes)=1439694848
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=798
        File Output Format Counters
                Bytes Written=7
```

Output Analysis
===============

```
[lhariharan142303@cxln4 ~]$ hdfs dfs -ls /user/lhariharan142303/wordcount/output/
Found 2 items
-rw-r--r--   3 lhariharan142303 lhariharan142303          0 2020-08-11 18:23 /user/lhariharan142303/wordcount/output/_SUCCESS
-rw-r--r--   3 lhariharan142303 lhariharan142303          7 2020-08-11 18:23 /user/lhariharan142303/wordcount/output/part-r-00000

[lhariharan142303@cxln4 ~]$ hdfs dfs -cat /user/lhariharan142303/wordcount/output/part-r-00000
hi      250
```