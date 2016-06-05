Nellie: spin up a virtual hadoop cluster with docker
====================================================

*Up! Down! Run! That's a good hadoop...*


Getting Started
---------------

If you have docker, docker-compose, and Python 3.x already installed on your system,
then cloning this repo and running the following command should be all you need to bring up a 2 node Hadoop cluster:

    ./nellie up

(In practice, you will usually interact with Nellie from one of your own project directories,
so future examples will omit the `./`,
and assume that you have already added the directory containing the nellie script to your PATH.
Note that actual installation, e.g. in `/usr/local/bin`, is not yet supported).

Use `nellie run` to issue commands against the cluster.
The following example uploads the README that you are currently reading to HDFS,
and uses hadoop streaming to grep for lines containing the word "nellie":

    nellie run hdfs dfs -mkdir grep.in
    nellie run hdfs dfs -put README.md grep.in
    nellie run hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -input grep.in -output grep.out -mapper 'grep nellie' -reducer 'cat'
    nellie run hdfs dfs -cat 'grep.out/*'

Notice that the reference above to README.md worked, even though the command is running under docker.
That is because your current working directory is automatically mounted as a docker volume at `/host`,
and that is set to docker's working directory.
In summary, things should Just Work if you avoid absolute paths, as well as relative paths that ascend to the parent directory.

To stop the cluster, type:

    nellie down


Web Interfaces
--------------

To be documented.


Scaling
-------

The nellie scale command allows scaling the virtual cluster to arbitrary size.
This functionality is still under development.
