# Slurm 

*Slurm* is a resource scheduler for HPC Clusters.

Slurm submission scripts are how you ask for resources on the cluster and what to do with those resources once you get them.

I have a collection of different examples I have used.
Its stored on [GitHub - Charles Forsyth](https://github.com/CharlesForsyth/slurm_scripts).

Select examples shown below.

## Spark Example

###### [spark_job.slurm](https://raw.githubusercontent.com/CharlesForsyth/slurm_scripts/master/spark_job.sh)

```bash
#!/bin/bash -l

#SBATCH -p short
#SBATCH --nodes=3
#SBATCH --cpus-per-task=8
#SBATCH --ntasks-per-node=1
#SBATCH --time=0:20:00
#SBATCH --job-name=spark-test

##########################################################
# PBS version was pulled from here:                      #
#   https://www.dursi.ca/post/spark-in-hpc-clusters.html #
##########################################################

nodes=($( scontrol show hostnames $SLURM_NODELIST ))
nnodes=${#nodes[@]}
last=$(( $nnodes - 1 ))

cd $SLURM_WORKING_DIR

export SPARK_HOME=/rhome/forsythc/bigdata/bin/spark
ssh ${nodes[0]}.ib.int.bioinfo.ucr.edu "module load java/8u45; cd ${SPARK_HOME}; ./sbin/start-master.sh"
sparkmaster="spark://${nodes[0]}:7077"

SCRATCH=/rhome/forsythc/bigdata/scratch/
mkdir -p ${SCRATCH}/work
rm -f ${SCRATCH}/work/nohup*.out

for i in $( seq 0 $last ); do
        ssh ${nodes[$i]}.ib.int.bioinfo.ucr.edu "cd ${SPARK_HOME}; module load java/8u45; nohup ./bin/spark-class org.apache.spark.deploy.worker.Worker ${sparkmaster} &> ${SCRATCH}/work/nohup-${nodes[$i]}.out" &
done

rm -rf ${SCRATCH}/wordcounts

cat > sparkscript.py <<EOF
from pyspark import SparkContext

sc = SparkContext(appName="wordCount")
file = sc.textFile("${SCRATCH}/moby-dick.txt")
counts = file.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a+b)
counts.saveAsTextFile("${SCRATCH}/wordcounts")
EOF

module load java/8u45
${SPARK_HOME}/bin/spark-submit --master ${sparkmaster} sparkscript.py

echo "Tried to use host name ${nnodes[0]} maybe we should use ${nodes[0]}"
ssh ${nodes[0]}.ib.int.bioinfo.ucr.edu "module load java/8u45; cd ${SPARK_HOME}; ./sbin/stop-master.sh"
for i in $( seq 0 $last ); do
    ssh ${nodes[$i]}.ib.int.bioinfo.ucr.edu "killall java"
    done
wait
```

