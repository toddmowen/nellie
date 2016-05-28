FROM cloudera/quickstart:5.7.0-0-beta

COPY nellie-worker /usr/bin/nellie-worker
COPY yarn-site.xml /etc/hadoop/conf/yarn-site.xml
COPY hdfs-site.diff /etc/hadoop/conf/hdfs-site.diff
RUN patch /etc/hadoop/conf/hdfs-site.xml /etc/hadoop/conf/hdfs-site.diff
RUN rm -rf /var/lib/hadoop-hdfs/cache/hdfs/dfs/data

COPY scala /usr/bin/scala
