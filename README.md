Nellie: spin up a virtual hadoop cluster with docker
====================================================

*Up! Down! Run! That's a good hadoop...*


KNOWN ISSUES: Nellie 0.1
------------------------

Nellie 0.1.0 has been released, with the following known issues:

- MapReduce jobs launched with `nellie run` fail. A temporary alternative,
  `nellie exec`, has been provided as a workaround. Usage is identical to
  `nellie run`, except that the working directory will be set to the location
  where the original `nellie up` command was issued, and *not* the location
  where the `nellie exec` command is issued.

- The `nellie jobtracker` command, which launches Chrome, will not correctly
  set the proxy configuration unless it is opening a new instance of Chrome,
  i.e. when Chrome is not already running.

- Job logs cannot be viewed via the web interface (an error page is displayed:
  "Failed while trying to construct the redirect url to the log server").
  The workaround is to use the yarn CLI, e.g.:

```
$ nellie exec yarn logs -applicationId application_1468148189971_0001
```


Getting Started
---------------

If you have docker, docker-compose, and Python 2.x already installed on your system,
then cloning this repo and running the following command should be all you need to bring up
a basic Hadoop "cluster" containing a single node:

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
    nellie run hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -input grep.in -output grep.out -mapper 'grep nellie' -reducer 'nl'
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
