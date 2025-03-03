## Code Project of Paper: Towards a Multi-Layer Intrusion Response System for Connected Vehicles

To see how the system is being used, either watch the video provided in the root folder of this repository (Demo.mp4) or read and test the documentation below.
You can read the paper (root folder of this repo 2202_Towar*.pdf) for further details.

## How to run the code:
- start system by jumping into the root folder and executing `docker-compose up`
- connect to the frontend of the system by using the browser and the url `localhost:5001`
- to connect to the rabbitMQ, use the tab of the frontend and login with the credentials `user` and `password`
- in the rabbit UI, you can inspect all connections, queues, channels, exchanges, etc.
- the swagger API is reachable via another UI tab but is kept at a minimum description
- to use the system:

		- NOTIFY TASK: 
			- for running the task notify, write `notify` into the manual response strategy field and hit `Execute Manual Response`
			- next go to the database tab and copy the job collection name and past it into the string field below and click `get data`, this shows successfull invocations of notify.
		- MANUAL TASK:
			- sends data to the response manager, write `manual_task` into the manual task field. the action fails if the response manager is offline.
		- REVOCATION OF TASK:
			- start a the manual task by writing `add` into the field. above in the monitoring of active response, you can see the job id. the add task runs for 15 seconds and is executed by a celery worker. to revoke the task, simply copy paste the job ID of the task into the revocation field in under manual response.
		- AUTOMATED RESPONSE:
			- to launch an automated response, click on `Sample 1` or `Sample 2` under the field `Automatic Response Strategies`
			- next click the yellow submit button to launch an automated response. the response will be handles asynchronously by celery workers and results are recorded in the database. rabbitMQ message brokers take over the task queueing and monitor/manage responses between the backend and the celery workers.
			- the output is visible in the event logs section below.

- possible tasks are: manual_task, notify, automated_task, add2, add, 
- other tasks can be added in the response-manager folder under the file `tasks.py`

#### docker connection string

- `mongo admin -u user -p "password"`

### Webserver stack
- container running the tornado webserver serving vue js frontend application
- portainer container setup to monitor docker containers via the UI

### RabbitMQ
Message broker for celery distributed tasking application on the containers

### Redis
Storing results of distributed tasking via celery in containers

### Data generator
Running and exposing software modules via REST API
- by calling the rest API it is possible to see the tools that can be executed in a celery process

Celery:
- start celery worker: `celery -A tasks worker --loglevel=info`

### licenses
- celerybeat-mongo: Apache-2.0 License
- celery: BSD-3-Clause License 
- tornado: Apache-2.0 License
- pymongo: Apache-2.0 License
- redis: BSD-3-Clause License
- pika: BSD-3-Clause License
- requests: Apache-2.0 License
