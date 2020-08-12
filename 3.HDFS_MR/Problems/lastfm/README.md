Problem Statement - Online Radio
================================
XYZ.com is an online music website where users listen to various tracks, the data gets collected like shown below. Write a map reduce program to get following stats

  * **Number of unique listeners**
  * **Number of times the track was shared with others**
  * **Number of times the track was listened to on the radio**
  * **Number of times the track was listened to in total**
  * **Number of times the track was skipped on the radio**
  
The data is coming in log files and looks like as shown below.
```
UserId|TrackId|Shared|Radio|Skip
111115|222|0|1|0
111113|225|1|0|0
111117|223|0|1|1
111115|225|1|0|0
```
Find out unique listeners per track.

First of all we need to understand the data, here the first column is UserId and the second one is Track Id. 
So we need to write a mapper class which would emit trackId and userIds and intermediate key value pairs. 
To make it simple to remember the data sequence, let's create a constants class as shown below

```
public class LastFMConstants {
 
    public static final int USER_ID = 0;
    public static final int TRACK_ID = 1;
    public static final int IS_SHARED = 2;
    public static final int RADIO = 3;
    public static final int IS_SKIPPED = 4;
     
}
Now, lets create the mapper class which would emit intermediate key value pairs as (TrackId, UserId) as shwon below

?
public static class UniqueListenersMapper extends
Mapper< Object , Text, IntWritable, IntWritable > {
        IntWritable trackId = new IntWritable();
        IntWritable userId = new IntWritable();
 
public void map(Object key, Text value,
    Mapper< Object , Text, IntWritable, IntWritable > .Context context)
        throws IOException, InterruptedException {
 
    String[] parts = value.toString().split("[|]");
    trackId.set(Integer.parseInt(parts[LastFMConstants.TRACK_ID]));
    userId.set(Integer.parseInt(parts[LastFMConstants.USER_ID]));
        if (parts.length == 5) {
        context.write(trackId, userId);
    } else {
        // add counter for invalid records
        context.getCounter(COUNTERS.INVALID_RECORD_COUNT).increment(1L);
    }
    }
}
```
                                        
You would have also noticed that we are using a counter here named INVALID_RECORD_COUNT , to count if there are any invalid records which are not coming the expected format. Remember, if we don't do this then in case of invalid records, our program might fail.



Now let's write a Reducer class to aggregate the results. Here we simply can not use sum reducer as the records we are getting are not unique and we have to count only unique users. Here is how the code would look like

```
public static class UniqueListenersReducer extends
    Reducer< IntWritable , IntWritable, IntWritable, IntWritable> {
    public void reduce(
        IntWritable trackId,
        Iterable< IntWritable > userIds,
        Reducer< IntWritable , IntWritable, IntWritable, 
        IntWritable>.Context context)
        throws IOException, InterruptedException {
        Set< Integer > userIdSet = new HashSet< Integer >();
        for (IntWritable userId : userIds) {
        userIdSet.add(userId.get());
        }
        IntWritable size = new IntWritable(userIdSet.size());
        context.write(trackId, size);
    }
}
```                                     
Here we are using Set to eliminate duplicate userIds. Now we can take look at the Driver class



```
public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        if (args.length != 2) {
            System.err.println("Usage: uniquelisteners < in > < out >");
            System.exit(2);
        }
        Job job = new Job(conf, "Unique listeners per track");
        job.setJarByClass(UniqueListeners.class);
        job.setMapperClass(UniqueListenersMapper.class);
        job.setReducerClass(UniqueListenersReducer.class);
        job.setOutputKeyClass(IntWritable.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
        org.apache.hadoop.mapreduce.Counters counters = job.getCounters();
        System.out.println("No. of Invalid Records :"
                + counters.findCounter(COUNTERS.INVALID_RECORD_COUNT)
                        .getValue());
    }
```
