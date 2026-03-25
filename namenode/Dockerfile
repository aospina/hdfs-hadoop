FROM apache/hadoop:3.4.3

USER root

ENV HADOOP_namenode_USER=root
ENV HADOOP_datanode_USER=root
ENV HADOOP_secondarynamenode_USER=root
ENV HADOOP_resourcemanager_USER=root
ENV HADOOP_nodemanager_USER=root

COPY config/core-site.xml /opt/hadoop/etc/hadoop/core-site.xml
COPY config/hdfs-site.xml /opt/hadoop/etc/hadoop/hdfs-site.xml
COPY config/mapred-site.xml /opt/hadoop/etc/hadoop/mapred-site.xml

RUN mkdir -p /hadoop/dfs/name

CMD ["/bin/bash", "-c", "\
  if [ ! -d /hadoop/dfs/name/current ]; then \
    hdfs namenode -format -force -nonInteractive; \
  fi && \
  hdfs namenode"]
