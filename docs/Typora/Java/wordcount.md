# WordCount源码分析

WordCount是一个*maven*工程

1. 首先需要在pom.xml里添加hadoop的相关依赖

1. 看到WordCount的代码部分，它的代码实现并不难，这里要编写3个类，分别是**map类，reduce类和主类**，map类和reduce类分别对应了MR编程模型中的mapping 阶段和reduce阶段

   1. 首先看到map类，它继承于MR框架的Mapper类，然后需要重写其中的map方法，接着看到map方法

      > 首先会取出文本中每行的数据，然后利用空格进行分词，最后通过一个循环将分词的结果写入到Mapper端的上下文中

   1. 然后看到Reduce类的实现，首先看到reduce方法的三个参数，分别是Map端输出的key值，Map端输出的value值的集合，以及reduce阶段的上下文。在reduce方法内

      > 会遍历values集合并把值相加，得到最终的词频数后写入reduce的上下文中。

   3. 最后是主类的代码部分

      ```java
      public static void main(String[] args) throws Exception {
      
          //新建了一个配置对象conf，可以配置mapreduce的一些参数
          Configuration conf = new Configuration();
      
          Job job = new Job(conf, "wordcount");
          //设置主类
          job.setJarByClass(WordCount.class);
          //设置Mapper类
          job.setMapperClass(Map.class);
          //设置Reducer类
          job.setReducerClass(Reduce.class);
      
          //设置输出数据的关键类
          job.setOutputKeyClass(Text.class);
          //设置输出值类
          job.setOutputValueClass(IntWritable.class);
      
      
          job.setInputFormatClass(TextInputFormat.class);
          job.setOutputFormatClass(TextOutputFormat.class);
      
          //设置输入和输出路径，由命令行参数来指定
          FileInputFormat.addInputPath(job, new Path(args[0]));
          FileOutputFormat.setOutputPath(job, new Path(args[1]));
      
          //开始工作
          job.waitForCompletion(true);
      }
      ```

