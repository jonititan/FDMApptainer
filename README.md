# FDMApptainer
An Apptainer recipe for using prefect on a HPC system for flight data analysis

https://docs.prefect.io/


To build the apptainer images

```
  $ sudo apptainer build fdm.sif fdm.def   
```
```                                     
  $ sudo apptainer build post.sif post.def 
```
  
To use
1. Create the folders needed
```
  $ mkdir pgrun
```
```
  $ mkdir pgdata
```

2. Start postgres server instance

Reference command for docker from [Prefect Docs](https://docs.prefect.io/2.10.3/concepts/database/#configuring-a-postgresql-database)

docker run -d --name prefect-postgres -v prefectdb:/var/lib/postgresql/data -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=yourTopSecretPassword -e POSTGRES_DB=prefect postgres:latest

our command
```
  $ apptainer instance start -B pgdata:/var/lib/postgresql/data -B pgrun:/var/run/postgresql -e -C --env-file post.envs post.sif  prefect-postgres
```

To access the instance
```
  $ apptainer shell -s /bin/bash instance://prefect-postgres
```

3. Run test
```
  $ apptainer run fdm.sif test.py
```

