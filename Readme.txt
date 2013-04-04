I. Data

There are three files in the directory "data":

1. nodes.txt contains the list of 101,852 nodes in the network. The first line is the total number of nodes. The second line is the time when these nodes are at risk. Here we assume this time is the beginning of the event window.

2. edges.txt contains the list of 4,793,160 edges in the network. The first line is the total number of edges. The second line is the time when these edges are already formed. Here we assume this time is the beginning of the event window.

3. allEvents.txt contains all tweets in the data set. The first line is the total number of tweets. Each tweet includes the timestamp, nodeID, and the label: 0 is for neutral; +1 is for positive; -1 is for negative; and -10 is for irrelevant.

II. Code

The provided code assumes that SAS is available in the running system. Coefficient estimates of the model in the paper can be obtained by running the following command:

java -Xmx10g -classpath twitter.jar:lib/commons-collections-3.2.1.jar exp/twitter/TwitterMisclassificationExperiment eventType outputDirectory sampleIndex controlSampleSize tweetData nodeData edgeData beginningTime endingTime

where 

1. eventType: 1 OR -1 for the estmation of positive OR negative tweeting behavior

2. outputDirectory: the directory where estimates and generated data sets are stored

3. sampleIndex: 200 estimates can be obtained by varying this parameter from 1 to 200

4. controlSampleSize: the control sample size which should be as large as possible. In the paper the control sample size is 3200.

5. tweetData: the tweet data file, i.e. allEvents.txt

6. nodeData: the node data file, i.e. nodes.txt

7 edgeData: the edge data file, i.e. edges.txt 

8 beginningTime: the beginning of the observation event window

9 endingTime: the ending of the observation event window. In the paper, the observation window of last 45 days is used.

For example, two following commands will obtain coefficient estimates of positive and negative tweeting behaviors for the sample 1 during the last 45-day observation window. Results are stored in estimates directory.

java -Xmx10g -classpath twitter.jar:lib/commons-collections-3.2.1.jar exp/twitter/TwitterMisclassificationExperiment  1 estimates 1 3200 data/allEvents.txt data/nodes.txt data/edges.txt 14583.43 14628.43

java -Xmx10g -classpath twitter.jar:lib/commons-collections-3.2.1.jar exp/twitter/TwitterMisclassificationExperiment -1 estimates 1 3200 data/allEvents.txt data/nodes.txt data/edges.txt 14583.43 14628.43

III. Notes

1. If the current system supports the job scheduling command qsub, multiple sample experiments can be generated using TwitterMisclassificationQSubGenerator command. 

java -classpath twitter.jar:lib/commons-collections-3.2.1.jar exp/twitter/TwitterMisclassificationQSubGenerator  1 estimates 200 3200 data/allEvents.txt data/nodes.txt data/edges.txt 14583.43 14628.43

java -classpath twitter.jar:lib/commons-collections-3.2.1.jar exp/twitter/TwitterMisclassificationQSubGenerator -1 estimates 200 3200 data/allEvents.txt data/nodes.txt data/edges.txt 14583.43 14628.43

2. The estimation can take a while since this legacy implementation does not compute network statistics on the fly. Faster implementation should only compute network statistics for sampled event and control nodes.

