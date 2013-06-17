JAVAC     = javac
JAR       = jar
OUT       = out
JAVA_SRC  = src/main/java
HDFS_OUT  = mrdp_out
INPUT_XML = foo.xml

all: mrdp.jar

MRDPUtils.class: $(JAVA_SRC)/mrdp/utils/MRDPUtils.java
	javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

CommentWordCount.class: $(JAVA_SRC)/mrdp/ch1/CommentWordCount.java
CommentWordCountIntSumReducer.class: $(JAVA_SRC)/mrdp/ch1/CommentWordCount.java
CommentWordCountSOWordCountMapper.class: $(JAVA_SRC)/mrdp/ch1/CommentWordCount.java
	javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

ch1classes: MRDPUtils.class CommentWordCount.class CommentWordCountIntSumReducer.class CommentWordCountSOWordCountMapper.class

AverageDriver.class: $(JAVA_SRC)/mrdp/ch2/AverageDriver.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

CountNumUsersByStateDriver.class: CountNumUsersByStateDriver.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

MedianStdDevDriver.class: MedianStdDevDriver.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

MinMaxCountDriver.class: MinMaxCountDriver.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

SmarterMedianStdDevDriver.class: SmarterMedianStdDevDriver.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

WikipediaIndex.class: $(JAVA_SRC)/mrdp/ch2/WikipediaIndex.java
        javac -sourcepath $(JAVA_SRC) -d $(OUT) -cp /usr/lib/hadoop/client/*:. $<

mrdp.jar: ch1classes
	cd $(OUT) && jar cvf mrdp.jar mrdp/utils/MRDPUtils.class mrdp/ch1/CommentWordCount.class mrdp/ch1/CommentWordCount\$$IntSumReducer.class mrdp/ch1/CommentWordCount\$$SOWordCountMapper.class && cd ..

hdfs_inputs:
	hadoop fs -put res/$(INPUT_XML) hdfs://dc1-util-hadoop1:8020/user/paul/$(INPUT_XML)

run:	clean clean_hdfs_inputs mrdp.jar hdfs_inputs
	HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce/ hadoop jar $(OUT)/mrdp.jar mrdp.ch1.CommentWordCount hdfs://dc1-util-hadoop1:8020/user/paul/$(INPUT_XML) $(HDFS_OUT)

clean:
	rm -rf $(OUT)/* $(HDFS_OUT)

clean_hdfs_inputs:
	hadoop fs -rmr hdfs://dc1-util-hadoop1:8020/user/paul/$(INPUT_XML)
