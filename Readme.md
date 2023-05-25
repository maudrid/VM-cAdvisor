# Victoria Metrics and cAdvisor issue recreated
## Versions
Docker version 24.0.1, build 6802122  
Docker Compose version v2.17.2  
cadvisor v0.47.1  
victoria-metrics v1.91.0  
prom/prometheus v2.43.0  
grafana 9.4.3

## Instructions
Deploy using `docker-compose up`  
Navigate to `http://localhost:3000`  
Log in with `test` / `test`  
Then go to `explore`  
Make sure to select `VM` as source.  
Enter this query `count({job=~"cadvisor"}) by (instance)`  
Select `last 30 minutes` as time span.  
Give it some time to accumulate data and note the following:
* The graph is not stable.
* Repeatedly clicking on `Run Query` changes the previously displayed graph values.
Change the source and now select `Prometheus` instead and execute the query again.
Note:
* The graph is stable
* Repeatedly clicking on `Run Query` does NOT chang the previously displayed graph values.


Now:  
Do `docker-compose down`  
Edit the docker-compose.yml file and rename the service called `cadvisorb` to `cadvisor`.  
Edit the prometheus.yml and change the target from `cadvisorb:8081` to `cadvisor:8081`.  
Run `docker-compose up`  
Follow the previous instructions in grafana using the `VM` data source and note:
* The graph is now stable and comparable to prometheus.
