# AWS

```bash
# only trip_fare_1

>> hadoop jar task1.jar -r 8 s3://chrisjermainebucket/comp330_A3/trip_fare_1.csv task1_seg1_out

>> hadoop jar task2.jar -r 8 s3://chrisjermainebucket/comp330_A3/trip_fare_1.csv task2_seg1_out

>> hadoop jar task3.jar -r 8 task2_seg1_results.txt task3_seg1_out

# all data

>> hadoop jar task1.jar -r 16 s3://chrisjermainebucket/comp330_A3/ task1_out

>> hadoop jar task2.jar -r 16 s3://chrisjermainebucket/comp330_A3/ task2_out

>> hadoop jar task3.jar -r 16 task2_results.txt task3_out
```



### Useful Commands

18.219.229.154

#### SFTP

```bash
# upload JAR via sftp
>> put /Users/Peter/Desktop/comp330/assignments/A3/a3pjct/out/artifacts/A3_jar/a3pjct.jar
```

#### Jar Paths

```bash
# Task 1
put /Users/Peter/Desktop/comp330/assignments/A3/a3pjct/out/artifacts/A3_jar/a3pjct.jar

# Task 2 Part 1
put /Users/Peter/Desktop/comp330/assignments/A3/a3task2part1/out/artifacts/a3t2p1_jar/a3task2part1.jar

# Task 2 Part 2
put /Users/Peter/Desktop/comp330/assignments/A3/a3task2part2/out/artifacts/a3t2p2_jar/a3task2part2.jar
```



#### SSH

```bash
# list everything in the hadoop file system
>> hadoop fs -ls

# delete directory "output-dir" from the file system
>> hadoop fs -rm -r output-dir

# run jar on small-subset.csv. output to output-dir
>> hadoop jar a3pjct.jar -r 8 small-subset.csv output-dir

# will concatenate all output files from reducer into a single file
>> cat output-dir/part-r-* > some-name 
```



### EMR - Elastic Map Reduce

1. Press "Create Cluster"

2. Choose “Go to advanced options”.

3. Default is fine. Click next.

4. On the next page, all of the defaults should be OK. Good options:

   Master Node: 1 x <u>m4.large machine</u>. 

   For the Core workers, you want 2 x <u>m4.large machines</u>. Click next.

   *If you are interested, you can find a list of all instance types at https://aws.amazon.com/ec2/instance-types/. Each m4.large machine has 4 CPU cores and 8GB of RAM.* 

5. On the next page (“General options”) everything should be OK, except you can unclick “termination protection”. Click next.

6. On the next page, under EC2 key pair, it is really important to choose the EC2 key pair that
   you just created. This is important: if you do not do this, you won’t be able to access your
   cluster.

7. Click “Create Cluster”. Now your machines are being provisioned in the cloud! 

### JAR

Build JAR:

```
At this point, go to Build -> Build Artifacts... -> [yourJarName].jar -> Build
```

- https://d1b10bmlvqabco.cloudfront.net/attach/j6lhc0u98a2x8/ioln360gey34l/j8ellicd91j0/jar_guide.pdf
- https://piazza.com/class/jl247sf8wky7me?cid=66

### SSH Into Master Node

From the directory containing your `.pem` file. Note IP depends on instance.

```bash
ssh -i "MyFirstKeyPair.pem" hadoop@54.172.82.0
```

### STFP Into Master Node

```bash
sftp -i "MyFirstKeyPair.pem" hadoop@54.172.82.0
```

Then type `put myJar.jar` to upload your jar. Type “exit” to exit the program.

### Load DATA via SSH

```bash
wget https://s3.amazonaws.com/chrisjermainebucket/text/Holmes.txt 
wget https://s3.amazonaws.com/chrisjermainebucket/text/dictionary.txt 
wget https://s3.amazonaws.com/chrisjermainebucket/text/war.txt
wget https://s3.amazonaws.com/chrisjermainebucket/text/william.txt
more Holmes.txt
```

### Enable SSH on Master Node

One your cluster is up and running, you will want to connect to the master node so that you run Hadoop jobs on it. Click “Cluster list”, and then click on your cluster. First you need to make it so that you can connect via SSH. Under "Security and Access" (not a tab on the side, just a heading at the lower right), go to "Security groups for Master" and click on the link. In the new page, click on the row with Group Name = "ElasticMapReduce-master”. At the bottom, click on the Inbound tab. Click on "Edit”. Click "Add Rule”. Then select “SSH” in the first box and “Anywhere” in the second. Click save. 

### Find IP

Now you need to connect. Again go to your cluster Under “Hardware”, you will see a list of
“Instance Groups”... you will have two types of instance groups in your cluster... a “core” group
(the workers) and the “master” group, which contains the machine that you will interact with.
Click on the ID associated with the “master” group, and you will see a clickable link under “EC2
Instance Id”. This is your master machine. Click on this link, and you will be taken to the EC2
dashboard where it will give you all sorts of info about your master node (if you ever want to get back to your cluster, just click on the cube at the upper left of the console, which gets you back to the main menu; click EMR, and then choose your cluster and you will be back to your cluster once again). The thing that we are really interested in is the public IP. This will be a number such as 54.172.82.0.



