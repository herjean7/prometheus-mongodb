## Prometheus MongoDB Alerts

#### Steps to Integrate your MongoDB Cluster with Prometheus

https://www.mongodb.com/docs/cloud-manager/tutorial/prometheus-integration/

Below are some alerts you can use to monitor your MongoDB cluster on Prometheus

##### MongoDB Cluster Storage Less Than 10%
    
```yaml
      - alert: MongodbStorage
        expr: ((hardware_disk_metrics_disk_space_free_bytes / (hardware_disk_metrics_disk_space_used_bytes{} + hardware_disk_metrics_disk_space_free_bytes{})) * 100) < 10
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Storage (instance {{ $labels.instance }})"
          description: "MongoDB Storage Less Than 10% \n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"          
```

##### MongoDB Read Tickets Less than 10 (Note. MongoDB 7.0 onwards employ Dynamic Ticketing System)
    
```yaml
      - alert: MongodbReadTickets
        expr: mongodb_wiredTiger_concurrentTransactions_read_available < 10
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Read Tickets (instance {{ $labels.instance }})"
          description: "MongoDB Read Tickets Less Than 10 \n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"          
```

##### MongoDB Write Tickets Less than 10 (Note. MongoDB 7.0 onwards employ Dynamic Ticketing System)
    
```yaml
      - alert: MongodbReadTickets
        expr: mongodb_wiredTiger_concurrentTransactions_write_available < 10
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Write Tickets (instance {{ $labels.instance }})"
          description: "MongoDB Write Tickets Less Than 10 \n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"          
```


##### MongoDB Replica Set Member Oplog Window Below 1 Hour
    
```yaml
      - alert: MongodbOplogWindowBelow1Hour
        expr: (mongodb_oplog_latestOptime-mongodb_oplog_earliestOptime) < 3600
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Oplog Window Less than an hour (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member Oplog Window Below 1 hour \n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"          
```

##### Replication Lag more than 10 seconds
    
```yaml
      - alert: MongodbReplicationLag
        expr: avg(mongodb_members_optime_ts{rs_state="1"}) - avg(mongodb_members_optime_ts{rs_state="2"}) > 10
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Replication Lag (instance {{ $labels.instance }})"
          description: "Mongodb replication lag is more than 10s\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"      
```
#### Details on Replica Set Member States

https://www.mongodb.com/docs/manual/reference/replica-states/

##### MongoDB Replica Set Member State = Recovering
    
```yaml

      - alert: MongodbReplicationStatus3
        expr: mongodb_members_state == 3
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB replication Status 3 (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member either perform startup self-checks, or transition from completing a rollback or resync\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"       
```

##### MongoDB Replica Set Member State = UNKNOWN
    
```yaml
      - alert: MongodbReplicationStatus6
        expr: mongodb_members_state == 6
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB replication Status 6 (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member as seen from another member of the set, is not yet known\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"
    
```

##### MongoDB Replica Set Member State = DOWN
    
```yaml
      - alert: MongodbReplicationStatus8
        expr: mongodb_members_state == 8
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB replication Status 8 (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member as seen from another member of the set, is unreachable\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"
 
```


##### MongoDB Replica Set Member State = ROLLBACK
    
```yaml
       - alert: MongodbReplicationStatus9
        expr: mongodb_members_state == 9
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB replication Status 9 (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member is actively performing a rollback. Data is not available for reads\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"    
          
```

##### MongoDB Connections more than 10k
    
```yaml
      - alert: MongodbConnections
        expr: mongodb_connections_current > 10000
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "MongoDB Connections (instance {{ $labels.instance }})"
          description: "MongoDB Replication set member Connections above 10k\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"     
          
```     
