## Levantamos contenedor con .sh (no es lo mas comun). Se suele hacer con archivos docker-compose.yml 

##### 1. Crear red para el cluster de hadoop

```
sudo docker network create --driver=bridge hadoop
```

##### 2. Inicializar el cluster.
```
cd DS-M4-Cluster_Hadoop
```

##### 2. Ejecutamos el archivo start-container.sh que esta aca en el repositorio. Con el comando de aqui abajo ya ejecutamos todo ese script. "./" para ejecutar 

```
sudo ./start-container.sh
```

**El output que vamos a obtener es el siguiente:
Aca ya estamnos dentro del hadoop-master**

```
start hadoop-master container...
start hadoop-slave1 container...
start hadoop-slave2 container...
root@hadoop-master:~# 
```
- Iniciar con 2  esclavos y un maestro
- Entraremos al contenedor master

##### 3. Iniciar hadoop - generamos la conexion con hadoop - inicializa hadoop

```
./start-hadoop.sh
```

##### 4. Con el comando wget traigo un archivo txt de un libro

```
wget https://raw.githubusercontent.com/uracilo/testdata/master/odisea.txt
```

##### 5. Crear un directorio

```
mkdir input
```

##### 6. Crear un archivo tipo tar.gz

```
tar -czvf input/odisea.tar.gz odisea.txt
```

-c: Generar archivo
-z: Comprimir con gzip.
-v: Progreso del proceso.
-f: Especificar nombre del archivo.


##### 7. Revisar los tamaños de nuestros archivos

```
ls -flarts input
```
##### 8. Crear y mover  directorio input al DFS de  HADOOP

```
hdfs dfs -mkdir -p test
hdfs dfs -put input
```

##### 9. Revisar nuestro input directorio en HADOOP

```
hdfs dfs –ls input
```

##### 10. Leer las primeras lineas de nuestro archivo en HADOOP

```
hdfs dfs -cat  input/odisea.tar.gz | zcat | tail -n 20
```

##### 11. Eliminar el archivo en HADOOP

```
hdfs dfs –rm –f /user/rawdata/example/odisea.tar.gz
```

##### Plus ejecutar un trabajo en HADOOP

```
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.7.2-sources.jar org.apache.hadoop.examples.WordCount input output
```

##### Plus ver el resultado del trabajo en HADOOP

```
hdfs dfs -cat output/part-r-00000
```

Inspirado en https://github.com/kiwenlau/hadoop-cluster-docker
