## Task 1
Spinning up 3 datanodes and 1 namenode running hadoop fs
#### Login to datanode
```sh
# [host]
docker exec -it hadoop-practicals-datanode-1 bash
```
#### Sending file from [local] to [container]
```sh
# docker cp <file(local)> <docker-container-name>:/path/to/file(container)
# for example
# [host]
docker cp ./testdata/people-1000000.csv hadoop-practicals-datanode1-1:/opt/hadoop/people-1000000.csv
```

#### Sending file from [container] to [hdfs]
```sh
# [container]
hdfs dfs -put ./people-1000000.csv /user/lordvidex/tests/
```

#### Confirm in UI
[File Explorer Link](http://localhost:9870/explorer.html#/user/lordvidex/tests)

--- 

## Task 2
Running map-reduce in hadoop fs setup for large files
> Some of the files can be gotten from open source repos like https://github.com/datablist/sample-csv-files
#### From Task 1, file should be in hdfs
```sh
# [container]
hdfs dfs -ls /user/lordvidex/tests/
```
#### Run hadoop sample map-reduce jar for wordcount (simple txt file)
```sh
# [container]
hadoop jar hadoop-mapreduce-examples-3.3.6.jar wordcount /user/lordvidex/tests/big.txt /user/lordvidex/results/big
```

#### Run hadoop sample map-reduce stream (large csv)
```sh
# small test
# [container]
hadoop jar hadoop-streaming-3.3.6.jar -file python/mapper.py -mapper python/mapper.py \
-file python/reducer.py -reducer python/reducer.py \
-input /user/lordvidex/tests/small.csv \
-output /user/lordvidex/results/small-stream
# large test
# [container]
hadoop jar hadoop-streaming-3.3.6.jar -file python/mapper.py -mapper python/mapper.py \
-file python/reducer.py -reducer python/reducer.py \
-input /user/lordvidex/tests/people-1000000.csv \
-output /user/lordvidex/results/people_csv-stream
```

#### Confirm in UI
[Cluster Info Link](http://localhost:8088/cluster/apps/FINISHED)
[Result files Directory](http://localhost:9870/explorer.html#/user/lordvidex/results)
