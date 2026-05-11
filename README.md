# Despliegue del clúster

Primera vez:
    
    docker compose build --no-cache

regularmente:

    docker compose up -d

para bajar:

    docker compose down

# Verificar contenedores:
    docker ps

Debe mostrar:

    namenode
    datanode1
    datanode2
    datanode3

# Acceso al NameNode

    docker exec -it namenode bash
    hdfs dfsadmin -report

# Operaciones básicas en HDFS
    hdfs dfs -ls /
    hdfs dfs -mkdir /data

# Experimento 1: Particionamiento 

# crear un archivo de 2GB

    dd if=/dev/urandom of=bigfile.txt bs=1M count=2000

# Subir a HDFS

    hdfs dfs -put bigfile.txt /data

# analizar particionamiento y replicación

    hdfs fsck /data/bigfile.txt -files -blocks -locations

# visualizar bloques 

    hdfs dfs -stat %b /data/bigfile.txt

# Ver tamaño de bloques:

    hdfs getconf -confKey dfs.blocksize

# Cambiar replicación a 3:
    
    hdfs dfs -setrep -w 3 /data/bigfile.txt

# Verificar:

    hdfs fsck /data/bigfile.txt -blocks -locations

verán:

    Replica on datanode1
    Replica on datanode2
    Replica on datanode3

# Simular falla de nodo

Detener nodo:
    
    docker stop datanode2

Revisar estado:

    hdfs dfsadmin -report

HDFS detecta la falla y replica los bloques.

# Preguntas para estudiantes

## Particionamiento
¿Cuántos bloques se generaron para el archivo?
    
    hdfs fsck
    
## Replicación
¿Cuántas réplicas tiene cada bloque?

## Ubicación
¿En qué nodos se almacenan?

## Tolerancia a fallos
¿Qué ocurre si se apaga un DataNode?

## Balanceo
¿HDFS intenta distribuir bloques equitativamente?
