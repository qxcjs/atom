[TOC]
## spark常用算子
测试数据
```scala
private val sparkSession = SparkSession.builder().master("local[*]").appName("ActionTest").getOrCreate()

private val rdd = sparkSession.sparkContext.parallelize(Seq("hello world","hello spark","hello hive","hello scala","hello java"))
```
### Transformation
#### coalesce
```scala
def coalesce(numPartitions: Int,shuffle: Boolean,partitionCoalescer: Option[org.apache.spark.rdd.PartitionCoalescer])(implicit ord: Ordering[String]): org.apache.spark.rdd.RDD[String]
```
该函数用于将RDD进行重分区，使用HashPartitioner。
第一个参数为重分区的数目，第二个为是否进行shuffle，默认为false;
如果重分区的数目大于原来的分区数，那么必须指定shuffle参数为true，//否则，分区数不变

#### repartition
```scala
def repartition(numPartitions: Int)(implicit ord: Ordering[String]): org.apache.spark.rdd.RDD[String]
```
该函数其实就是coalesce函数第二个参数为true的实现。

#### flatMap
flatMap函数则是两个操作的集合——正是“先映射后扁平化”：
操作1：同map函数一样：对每一条输入进行指定的操作，然后为每一条输入返回一个对象
操作2：最后将所有对象合并为一个对象
```scala
def flatMap[U](f: String => TraversableOnce[U])(implicit evidence$4: scala.reflect.ClassTag[U]): org.apache.spark.rdd.RDD[U]
```
第一个参数是一个集

#### glom
```scala
def glom(): org.apache.spark.rdd.RDD[Array[String]]
```
该函数是将RDD中每一个分区中类型为T的元素转换成Array[T]，这样每一个分区就只有一个数组元素。

#### union
该函数比较简单，就是将两个RDD进行合并，不去重。

#### intersection
该函数返回两个RDD的交集，并且去重。
```scala
def intersection(other: org.apache.spark.rdd.RDD[String],numPartitions: Int): org.apache.spark.rdd.RDD[String]

def intersection(other: org.apache.spark.rdd.RDD[String],partitioner: org.apache.spark.Partitioner)(implicit ord: Ordering[String]): org.apache.spark.rdd.RDD[String]

def intersection(other: org.apache.spark.rdd.RDD[String]): org.apache.spark.rdd.RDD[String]
```
参数numPartitions指定返回的RDD的分区数。
参数partitioner用于指定分区函数

### subtract
```scala
def subtract(other: org.apache.spark.rdd.RDD[String],p: org.apache.spark.Partitioner)(implicit ord: Ordering[String]): org.apache.spark.rdd.RDD[String]

def subtract(other: org.apache.spark.rdd.RDD[String],numPartitions: Int): org.apache.spark.rdd.RDD[String]

def subtract(other: org.apache.spark.rdd.RDD[String]): org.apache.spark.rdd.RDD[String]
```
该函数类似于intersection，但返回在RDD中出现，并且不在otherRDD中出现的元素，不去重。
参数含义同intersection。



### Action
