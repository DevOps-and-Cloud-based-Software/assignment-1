% RESTful services, docker and Kubernetes



# Introduction

  In this tutorial we will use OpenAPI to define a RESTful web service and Python to implement it.  

The RESTful web service will be using a database to store data. 

More specifically the steps of this tutorial are the following: 

1. Write OpenAPI definition using SwaggerHub
2. Generate the service stubs in Python and implement the logic
3. Build Test and Publish Docker Image 
4. Write Tests
5. Deploy Web Service on Kubernetes (MicroK8s)


# Reporting and assessment

## Reporting
At the end of this assignment you are expected to submit the following:

  * Each student should write a short report (max 4 pages) with the following structure: 
    + **Introduction**: List which of the DevOps stages you practiced with this assignment and what are their primary objectives.
    + **OpenAPI Exercises**: Report on the following exercises (for details see on the Section  [OpenAPI Exercises](#openapi-exercises)):
      - [Define Objects](#define-objects)
      - [Add Delete method](#add-delete-method)
    + **[MongoDB Integration (Optional)](#mongodb-integration--optional-)**: If you choose to integrate an external database report on how the modifications you had to make on your code and Dockerfile to connect to DB.
    + **Question 1**: In the Docker file we use the 'python:3.9-alpine' image as a base image. What are the advantage and disadvantages of using the Alpine Linux distribution in Docker.

## Assessment

---

 **IMPORTANT**

 In order to be given a grade you must submit the following:

 * Written report (see [Reporting](#reporting) for details)
 * Name of the published docker(s) in [https://hub.docker.com/](https://hub.docker.com/). Must be able to perform (docker pull <REPO/NAME>)
 * Git repository link
 * If you choose the optional exercise task i.e. the MongoDB integration submit **only** the code for connecting to MongoDB. 
 * If you choose the optional exercise task i.e. the MongoDB integration the testing will be done using the '[docker-compose.yaml](https://raw.githubusercontent.com/DevOps-and-Cloud-based-Software/week1/main/docker-compose.yaml)'

 **Do not add your code in Canvas or in your report.**

 **All links such as DockerHub and Git must be accessible from the day of submission and onwards.**

---


* If the source code and dockerized service pass all the tests defined in newman you get 60%.
* A passing mark will be given if the source code and dockerized service pass tests 1-5 in newman (see Section [Build Test and Publish Docker Image](#build-test-and-publish-docker-image)).
* The tests will run on the docker created from source i.e. from building the docker file from source and the docker image from your DockerHub account. 
* An extra point will be given for the [MongoDB Integration (Optional)](#mongodb-integration--optional-).
* The rest will be determined by your report. 

# Background

## OpenAPI and Swagger
Swagger is an implementation of OpenAPI. Swagger contains a tool that helps developers design, build, document, and consume RESTful Web services. 
Applications implemented based on OpenAPI interface files can automatically generate documentation of methods, parameters and models. This helps keep the documentation, client libraries, and source code in sync.
You can find a short technical explanation here [https://www.youtube.com/watch?v=pRS9LRBgjYg](https://www.youtube.com/watch?v=pRS9LRBgjYg)

## Git
Git is an open source distributed version control system. Version control helps keep track of changes in a project and allows for collaboration between many developers.
You can find a short technical explanation here https://www.youtube.com/watch?v=wpISo9TNjfU

## GitHub Actions 
GitHub Actions automates your software development workflows from within GitHub. In GitHub Actions, a workflow is an automated process that you set up in your GitHub repository. You can build, test, package, release, or deploy any project on GitHub with a workflow.

## Docker
Docker performs operating-system-level virtualization, also known as "containerization". Docker uses the resource isolation features of the Linux kernel to allow independent "containers" to run within a Linux instance.
You can find a short technical explanation on containerization here https://www.youtube.com/watch?v=0qotVMX-J5s


## Kubernetes (MicroK8s)
Kubernetes is an open-source container orchestration system for automating software deployment, scaling, and management. 
You can find a short technical explanation on container orchestration here https://www.youtube.com/watch?v=kBF6Bvth0zw


# Prepare your Development Environment 

## Create GitHub Account
If you don’t have an account already follow these instructions: https://github.com/join 

## Setup Docker Hub
If you don’t have an account already follow these instructions: https://hub.docker.com/signup 

## SwaggerHub Account
If you have GitHub Account you may go to https://app.SwaggerHub.com/login and select 'Log In with GitHub'. Alternatively, you can select to sign up.

## Install Docker and Docker Compose on your Local machine
You can find instructions on how to install Docker here: https://docs.docker.com/get-docker/.
You may also find a detailed tutorial on Docker here: [https://docker-curriculum.com/](https://docker-curriculum.com/).

To test if your installation is running you may test docker by typing:
```shell
docker run hello-world
```
You can find instructions on how to install Docker Compose here: 
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/).

## Install Pycharm 
In this tutorial we will use the Pycharm Integrated Development Environment (IDE). If you have a 
preferred IDE you are free to use it.

You can find instructions on how to install Pycharm here: 
[https://www.jetbrains.com/pycharm/download/](https://www.jetbrains.com/pycharm/download/)

If you are using snap you can type:
```shell
sudo snap install pycharm-community --classic
```
You may also find a detailed tutorial on Pycharm here: 
https://www.jetbrains.com/help/pycharm/creating-and-running-your-first-python-project.html

# Write OpenAPI Definition
In this section we will define a web service interface that will support the Create, Read, Update, Delete (CRUD) pattern 
for manipulating resources using OpenAPI. To get a more in-depth understanding of Swagger and OpenAPI you may follow this tutorial 
[https://idratherbewriting.com/learnapidoc/openapi_tutorial.html](https://idratherbewriting.com/learnapidoc/openapi_tutorial.html)

Log in to your SwaggerHub account at [https://app.SwaggerHub.com/login](https://app.SwaggerHub.com/login) and select 'Create New' -> 'Create New API'

<img src="/images/swhub1.png" alt="swagger" width="800"/>

Then select version 3.0.0 and 'Template' 'Simple API' and press 'CREATE API'.

<img src="/images/swgub2.png" alt="swagger" width="800"/>

You will get a OpenAPI template

<img src="/images/swgub3.png" alt="swagger" width="800"/>

Name your API 'tutorial'.


Replace the definition with the following:

```YAML:openAPI_1.yaml

```



You will notice that the editor at the bottom throws some errors:
```
Errors (3)
Could not resolve reference: #/components/schemas/Student
$refs must reference a valid location in the document
$refs must reference a valid location in the document
```

Effectively what is said here is that the "#/components/schemas/Student" is not defined. 
You can find more about '$refs' here: [https://swagger.io/docs/specification/using-ref/](https://swagger.io/docs/specification/using-ref/)

## OpenAPI Exercises

### Define Objects
Scroll down to the bottom of the page and create a new node called 'components', a node 
'schemas' under that and a node called 'Student' under that. The code should look like this:
```yaml
components:
  schemas:
    Student:
      type: object
      required:
        - first_name
        - last_name
      properties:
        ...
    GradeRecord:
      type: object
      required:
        - subject_name
        - grade
      properties:
        ...
```


Define the Student's and GradeRecord object properties. 

The Student properties to define are:

| Property Name | Type                    | 
|---------------|-------------------------|
| student_id    | number (integer format) | 
| first_name    | string                  |
| last_name     | string                  |
| gradeRecords  | array                   |

The GradeRecord properties to define are:

| Property Name | Type                                            | 
|---------------|-------------------------------------------------|
| subject_name  | string                                          | 
| grade         | number ( float format, minimum: 0, maximum: 10) |                                   |


---

 **NOTE**

 Take not on which properties are required and which are not. This will affect the services' validation process.
 To get more information on the 'required' see: https://swagger.io/docs/specification/data-models/data-types/ in the 
 'Required Properties' Section.

---



It is useful to add 'example' fields in the properties. That way you API is easier to consume.

You can find details about the 'example' field here: [https://swagger.io/docs/specification/adding-examples/](https://swagger.io/docs/specification/adding-examples/)
You can find details about data models here: [https://swagger.io/docs/specification/data-models/](https://swagger.io/docs/specification/data-models/)

### Add Delete method

The API definition at the moment only has 'GET' and 'POST' methods. We will add a 'DELETE' 
method. Under the '/student/{student_id}' path add the following:
```yaml
    delete:
      summary: gets student
      operationId: deleteStudent
      description: |
        delete a single student
      parameters:
string "do some magic!". 

## Create Git Repository and Commit the Code
Create a git repository. Go to the directory of the code and initialize the git repository and 
push the code:
```shell
git init
git add .
git commit -m "first commit"
git remote add origin <REPOSETORY_URL>
git push -u origin master
```
More information on git can be found here: [https://phoenixnap.com/kb/how-to-use-git](https://phoenixnap.com/kb/how-to-use-git)

### Implement the logic

In Pycharm create a package named 'service'. To do that right click on the 'swagger_server' package select 'New'->
'Python Package' and enter the name 'service'
<img src="/images/pych7.png" alt="swagger" width="800"/>

Inside the service package create a new python file named 'student_service'

In the student_service add the following code:
[student_service.py](https://gitlab-fnwi.uva.nl/skoulou1/devops-material/-/raw/main/python/student_servcie.py)
```python
import os
import tempfile

from tinydb import TinyDB, Query
from tinydb.middlewares import CachingMiddleware
from functools import reduce

from swagger_server.models import Student

db_dir_path = tempfile.gettempdir()
db_file_path = os.path.join(db_dir_path, "students.json")
student_db = TinyDB(db_file_path)


def add(student=None):
    queries = []
    query = Query()
    queries.append(query.first_name == student.first_name)
    queries.append(query.last_name == student.last_name)
    query = reduce(lambda a, b: a & b, queries)
    res = student_db.search(query)
    if res:
        return 'already exists', 409

    doc_id = student_db.insert(student.to_dict())
    student.student_id = doc_id
    return student.student_id


def get_by_id(student_id=None, subject=None):
    student = student_db.get(doc_id=int(student_id))
    if not student:
        return 'not found', 404
    student['student_id'] = student_id
    print(student)
    return student


def delete(student_id=None):
    student = student_db.get(doc_id=int(student_id))
    if not student:
        return 'not found', 404
    student_db.remove(doc_ids=[int(student_id)])
    return student_id
```

Now you can add the corresponding methods in the 'default_controller.py'. To do that add in the top of the file:

```python
from swagger_server.service.student_servcie import *
```

And in the 'add_student' student method we add the 'add(body)' in the rerun statement so now the method becomes :

```python
def add_student(body=None):  # noqa: E501
    """Add a new student

    Adds an item to the system # noqa: E501

    :param body: Student item to add
    :type body: dict | bytes

    :rtype: float
    """
    if connexion.request.is_json:
        body = Student.from_dict(connexion.request.get_json())  # noqa: E501
        return add(body)
    return 500,'error'
```

Do the same for the rest of the methods. For example, in the ‘delete_student’ method in the 'default_controller.py' file, you need to add ‘delete(student_id)’ in the return statement. 

In general, it is a good idea to write application using layered architecture. By segregating 
an application into tiers, a developer can modify or add a layer, instead of reworking the 
entire application.

This is why we should create a new package in the code called 'service' and a python file named
'student_service.py'. In this code template we use a simple file-based database to store and 
query data called TinyDB. 

More information on TinyDB can be found here: [https://tinydb.readthedocs.io/en/latest/getting-started.html](https://tinydb.readthedocs.io/en/latest/getting-started.html)
Now the 'default_controller.py' just needs to call the service's methods. 

---

 **NOTE**

 Don't forget to regularly commit and push your code.

---

# Build Test and Publish Docker Image 

You can now build your web service as a Docker image DockerHub. To do that open the Dockerfile 
in the Pycharm project.

and update the python version from:
```Dockerfile
FROM python:3.9-alpine
```
To:
```Dockerfile
FROM python:3.8-alpine
```

So the Dockerfile will look like this:
[Dockerfile](https://gitlab-fnwi.uva.nl/skoulou1/devops-material/-/raw/main/docker/Dockerfile)
```Dockerfile
FROM python:3.8-alpine

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY requirements.txt /usr/src/app/
    - name: Build docker
      run: |
        docker build . -t $IMAGE_ID
    - name: run docker
      run: |
        docker run -d -e 8080:8080 -host=172.17.0.2 $IMAGE_NAME:latest && \
          docker ps && sleep 5

    - name: Run API Tests
      id: run-newman
      uses: anthonyvscode/newman-action@v1
      with:
        collection: postman/postman_collection.json
        environment: postman/postman_environment.json
        reporters: cli
        iterationCount: 3

    - name: Output summary to console
      run: echo ${{ steps.run-newman.outputs.summary }}

    - name: Login to Container Registry
      uses: docker/login-action@v1
      with:
        
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}

    - name: Push image to docker hub Container Registry
      run: |
        docker push $IMAGE_ID
```

## MongoDB Integration (Optional)

# Deploy Web Service on Kubernetes (MicroK8s)

## Install MicroK8s 

If you have access to a Kubernetes cluster you may skip this step

You can find MicroK8s installation instructions: [https://MicroK8s.io/](https://MicroK8s.io/)

If you have access to a cloud VM you may install MicroK8s there. Alternatively you may also use 
VirtualBox: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

After you complete the installation make sure you start MicroK8s and enable DNS
```shell
MicroK8s start
MicroK8s enable dns
```

## Test K8s Cluster

This is a basic Kubernetes deployment of Nginx. On the master node create a Nginx deployment:
```shell
MicroK8s kubectl create deployment nginx --image=nginx
```

You may check your Nginx deployment by typing:
```shell
MicroK8s kubectl get all
```

The output should look like this:
```shell
NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-6799fc88d8-wttct   0/1     ContainerCreating   0          6s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   134d

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   0/1     1            0           7s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-6799fc88d8   1         1         0       7s
```
You will notice in the first line 'ContainerCreating'. This means that the K8s cluster is downloading and starting the 
Nginx container. After some minutes if you run again:
```shell
MicroK8s kubectl get all
```

The output should look like this:
```shell
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-6799fc88d8-wttct   1/1     Running   0          39s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   134d

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           40s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-6799fc88d8   1         1         1       40s
```

At this point Nginx is running on the K8s cluster, however it is only accessible from within the cluster. To expose 
Nginx to the outside world we should type: 
```shell
MicroK8s kubectl create service nodeport nginx --tcp=80:80
```

To check your Nginx deployment type:
```shell
MicroK8s kubectl get all
```

You should see the among others the line:
```shell
service/nginx        NodePort    10.98.203.181   <none>        80:30155/TCP   6s
```

This means that port 80 is mapped on port 30155 of each node in the K8s cluster. 

---

 **NOTE**

 The mapped port will be different on your deployment. Now we can access Nginx from 
 http://IP:NODE_PORT.

---



You may now delete the Nginx service by using:
```shell
MicroK8s kubectl delete service/nginx
```

## Deploy Web Service on K8s Cluster

To deploy a RESTful Web Service on the K8s Cluster log in the K8s Cluster create a folder named
'service' and add this file in the folder:
[student_service.yaml](https://gitlab-fnwi.uva.nl/skoulou1/devops-material/-/raw/main/k8s/student_service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    io.kompose.service: service
  name: service
spec:
  type: NodePort
  ports:
  - port: 8080
    protocol: TCP
    name: http
  selector:
    io.kompose.service: service
---
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.16.0 (0c01309)
  creationTimestamp: null
  labels:
    io.kompose.service: service
  name: service
spec:
  selector:
    matchLabels:
      io.kompose.service: service
  replicas: 1
  template:
    metadata:
      creationTimestamp: null
      labels:
        io.kompose.service: service
    spec:
      containers:
      - env:
        - name: MONGO_URI
          value: mongodb://mongo:27017
        image: IMAGE_NAME
        name: service
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
        resources: {}
      restartPolicy: Always
status: {}
```

Open 'student_service.yaml' and replace the line:
```yaml
image: IMAGE_NAME
```
with the name of your image as typed in the docker push command. 


If you chose to integrate with an extremal database you will need to add the Deployment and 
service for MongoDB:
[mongodb.yaml](https://gitlab-fnwi.uva.nl/skoulou1/devops-material/-/raw/main/k8s/mongodb.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose --file docker-compose.yml convert
    kompose.version: 1.16.0 (0c01309)
  labels:
    io.kompose.service: mongo
  name: mongo
spec:
  ports:
  - name: "27017"
    port: 27017
    targetPort: 27017
  selector:
    io.kompose.service: mongo
---
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.16.0 (0c01309)
  labels:
    io.kompose.service: mongo
  name: mongo
spec:
  selector:
    matchLabels:
      io.kompose.service: mongo
  replicas: 1
  template:
    metadata:
      labels:
        io.kompose.service: mongo
    spec:
      containers:
      - image: mongo:4
        name: mongo
      restartPolicy: Always
```

To create all the deployments and services type in the K8s folder:
```shell
MicroK8s kubectl apply -f .
```

This should create the my-temp-service deployments and services. To see what is running on the cluster type:
```shell
MicroK8s kubectl get all
```
You should see something like this:
```shell
NAME                           READY   STATUS    RESTARTS   AGE
pod/nginx-6799fc88d8-q7hzv     1/1     Running   0          21m
pod/service-6c75dff7db-57sw5   1/1     Running   0          53s

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.152.183.1     <none>        443/TCP          26m
service/service      NodePort    10.152.183.176   <none>        8080:30726/TCP   53s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx     1/1     1            1           21m
deployment.apps/service   1/1     1            1           53s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-6799fc88d8     1         1         1       21m
replicaset.apps/service-6c75dff7db   1         1         1       53s

```

Note that in this output 'service/service' is mapped to 30726. In your case it may be a different number.

Now your service should be available on http://IP:NODE_PORT/

