# oap_binary

# Data Source Usage
Please check the "Get started" section
https://github.com/Intel-bigdata/OAP/blob/master/oap-data-source/arrow/README.md

# Native SQL Engine Usage
https://github.com/Intel-bigdata/OAP/blob/master/oap-native-sql/README.md

# Q&A
1. For Native SQL Engine, there are some dependency library you need, please check the resource directory and copy the so file into your library directory.
You may have to create a soft link for those three files, below is an example to copy all three libraries into /usr/local/lib64 as a reference.
```
libarrow.so.17 => /usr/local/lib64/libarrow.so.17
libparquet.so.17 => /usr/local/lib64/libparquet.so.17
libgandiva.so.17 => /usr/local/lib64/libgandiva.so.17
```

To double check all the dependencies are installed, please check below commands andd see if every dependencies can be found:
```
jar xf spark-columnar-core-1.0.0-jar-with-dependencies.jar
ldd libspark_columnar_jni.so
```
2. Release Notes
Arrow data source notes:
* Better support for data source V1
* Allow to config batch size for data source V1
* Complete execution metrics on Spark UI
* New memory management system added. Executor level off-heap memory control is now available
* Experimental: Ability to completely replace Spark Parquet data source with low-level Arrow data source execution
* Fix file describer leakages

Columnar Shuffle notes:
* Improvements for the split operation in columnar shuffle using prefetch and AVX instructions
* AQE support for columnar shuffle 
* Reduce disk write in shuffle write
* Fix shuffle files not deleted properly
* Detailed metrics for columnar exchange operator

Columnar Compute Engine notes:
Notable changes in 1.0 release: 
* Columnar Sort: improvements for String based key, support different sort orders on multiple keys. Removed codegen time for single key based sort
* Columnar BroadcastHashJoin: building columnar hashmap on driver node, fix potential hash conflict by using whole key compare. Optimized memory usage in BroadcastHashJoin.  
* Columnar ShuffledHashJoin: fix potential hash conflict by using key comparison
* Initial Columnar Whole stage Codegen: supports combining Columnar ShuffledHashJoin, Columnar BroadcastHashJoin and Columnar Projections in one whole stage codegen stage. 
* AQE support: support for Columnar BroadcatsHashJoin
* Initial support for Columnar SortMergeJoin: supporting inner/semi/anti/outer joins.
* Support fallback to row-based execution:  automatically fallback to row-based execution if there are SortMergejoin, BroadcastHashjoin and Projection in one stage.  Automatically fallback to row-based execution there are unsupported  expressions/operators. 
* Columnar Window support
* Columnar Union/Expand support
Limitations:
* Limited support for decimal/timestamp type 
* Limited support for Columnar SortMergeJoin

