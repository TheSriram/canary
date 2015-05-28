
# Canary
Monitoring Service for Openstack Taskflow's Jobboards

# How does it work?
*Task-ception*

Canary is used to monitor other job-boards. The results are posted to MongoDB.

The dashboard uses meteor.js and will update in realtime, detecting changes in the MongoDB.

# Console Scripts

```python
canary-producer
```

Canary's own producer process that posts jobs to Canary's jobboard

```python
canary-worker
```

Canary's conductor which pulls jobs off Canary's jobboard, to query
paths submitted to it (using Taskflow) and post to cassandra with a timestamp

```python
canary-server
```

Canary's WSGI server that serves job counts and corresponding jobs, over the period
of last `x` seconds (which is set as interval under canary.conf)

# Dashboard

From the /dashboard folder, run:

```python
$ meteor 
```

# API

The API is optional.  Usually the canary-producer is run via cron.

```python

GET v1.0/jobs?path=/taskflow/jobs/myawesomejobs
  

HTTP/1.0 200 OK
Date: Fri, 27 Mar 2015 14:45:13 GMT
Server: WSGIServer/0.1 Python/2.7.6
content-type: application/json; charset=utf-8

[
    {
        "job_count": 1,
        "jobs": [
            {
                "_backend": {
                    "connection": "zookeeper"
                },
                "_book": null,
                "_book_data": {
                    "name": "myawesomeservice-2db7c7fb-e3d4-4602-8658-2c0ec0736b50",
                    "uuid": "81d22b6c-d11e-4c41-8e05-e423385a0ddc"
                },
                "_created_on": "2015-03-27T10:28:07.951000",
                "_details": {
                    "store": {
                        "awesome": "sauce",
                    }
                },
                "_lock_path": "/taskflow/jobs/myawesomejobs/job0000000042.lock",
                "_name": "myawesomeservice-2db7c7fb-e3d4-4602-8658-2c0ec0736b50",
                "_node_not_found": false,
                "_path": "/taskflow/jobs/myawesomejobs/job0000000042",
                "_root": "/taskflow/jobs/myawesomejobs/",
                "_sequence": 42,
                "status": "CLAIMED",
                "_uuid": "d76816c3-db93-4b66-8272-637b114a84e9"
            },
        ]
    }
]
```
