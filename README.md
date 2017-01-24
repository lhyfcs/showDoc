Autoscaling Simulation server
============

This is  the API service for autoscaling simulation.

The service now supports 2 features.

Generate jobs time data from source data.
----
    POST /api/1.0/getJobs 
    jobamanger: xxx.txt
    splunk: xxx.json
    
    HTTP/1.1 200 OK
    [
        {
            "renderingTime":29,
            "downloadTime":3,
            "scenebuilderTime":7,
            "zipTime":3,
            "type":"image",
            "startTime":86266
        }
        ....
    ]
Two input files are needed, one is txt record data from jobmanager, file name is like xxx.txt each line in this file have 4 columns, the are -- Jobid, create time, rendering type and job final status

    8a4f8bce-4bea-4a18-a0eb-da5eb302d69a,	0:21:24	,image,2
    
Another is record data from splunk, the file has .json file append, but is not a real json file, in this file, each line is a json object, 

    {
        "preview":false,
        "result":{
            "event_type":"RenderProxyEnd",
            "jobid":"63604c0c-1c1d-4af9-8f68-350d71b99aa9",
            "timestamp":"2016-11-28T15:59:16.170Z"
        }   
    }


Simulation
----
1. Simulation by jobs

        POST /api/1.0/simulationByJobs
        jobs: xxx.json
        options:
        {
            "maxMachine": 50,
            "groupCount": 2,
            "capacity": 3,
            "imageTime": 120,
            "panoramaTime": 500
        }
    
        HTTP/1.1 200 OK
        {
            "machine": [
                {
                    "hour": 20,
                    "living": 20,
                    "working": 20,
                    "id": "1",
                    "actions": [
                        {
                            "open": "2:10:10",
                            .....
                        }
                    ]
                }
                ....
            ],
            "useRate": 88,
            "jobs": [
                {
                    "totalTime": 100,
                    "type": "image",
                    "startTime": 100,
                    "endTime": 100,
                    "waitingTime": 200,
                    "raasTime": 200
                }
                ...
            ],
            "timerRecords": [
                {
                    "runningMachine": 100,
                    "runningJob": 4,
                    "waitingJob": 10
                }
                ...
            ],
            "averageRendering": 100,
            "averageWaiting": 100
        }
    
    Input json file, data is a json array, the file can get by API /api/1.0/getJobs or create by user themself.

        [
            {
                "renderingTime":218,
                "downloadTime":3,
                "scenebuilderTime":3,
                "zipTime":1,
                "type":"image",
                "startTime":1284
            }
            ....
        ]
        
        
        
2. Simulation by source data


        POST /api/1.0/simulationByData
        jobmanager: xxx.txt
        splunk: xxx.json
        options:
        {
            "maxMachine": 50,
            "groupMachineCount": 2,
            "reservedCapacity": 3,
            "imageRenderingTime": 120,
            "panoramaRenderingTime": 500,
            "recordTime": 120,
            "autoscalingTimer": 120
        }
    
        HTTP/1.1 200 OK
        {
            "machine": [
                {
                    "hour": 20,
                    "living": 20,
                    "working": 20,
                    "id": "1",
                    "actions": [
                        {
                            "open": "2:10:10",
                            .....
                        }
                    ]
                }
                ....
            ],
            "useRate": 88,
            "jobs": [
                {
                    "totalTime": 100,
                    "type": "image",
                    "startTime": 100,
                    "endTime": 100,
                    "waitingTime": 200,
                    "raasTime": 200
                }
                ...
            ],
            "timerRecords": [
                {
                    "runningMachine": 100,
                    "runningJob": 4,
                    "waitingJob": 10
                }
                ...
            ],
            "averageRendering": 100,
            "averageWaiting": 100
        }
        
    Two input files are needed, one is txt record data from jobmanager, file name is like xxx.txt each line in this file have 4 columns, the are -- Jobid, create time, rendering type and job final status

        8a4f8bce-4bea-4a18-a0eb-da5eb302d69a,	0:21:24	,image,2
    
    Another is record data from splunk, the file has .json file append, but is not a real json file, in this file, each line is a json object, 

        {
            "preview":false,
            "result":{
                "event_type":"RenderProxyEnd",
                "jobid":"63604c0c-1c1d-4af9-8f68-350d71b99aa9",
                "timestamp":"2016-11-28T15:59:16.170Z"
            }   
        }
    


