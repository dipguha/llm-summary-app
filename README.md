# llm-summary-app
LLM summary app


# Lecture 2: Installing development software
1. VS Code
2. Docker
3. Postman
4. Anaconda - virtual environment
5. AWS - chandradipguha@gmail.com
6. AWS CLI
```
conda create -n llmapplications
conda env list
conda activate llmapplications
cd <dir>
pip3 install -r requirements.txt
code .
install python if pop up appears
shift + command + p -> python: select interpreter - python(llmapplications)
```

# Lecture 3: Building a Prediction Pipeline
1. Get API key from OpenAI
2. app.py
3. import openai, os, longchain.text_splitter (CharacterTextSplitter)
4. text splitter and text chunks, each text chunk are stored in a list of documents
5. Define llm, map reduce and summary


# Lecture 4: Testing Prediction Pipeline
1. Define text1 = provide text
2. Call the function
```
python3 app.py
```

# Lecture 5: Installing AWS CLI
1. Access key and secret access key
2. Create user - IAM user - custom password
3. Install AWS CLI for Mac

# Lecture 6: Setting up Secrets Manager
1. Secrets Manager via Console - Store new secret - Other type of secret - key value - Name the secret key with description

# Lecture 7: Injecting API key from SM into App
1. SM returns as string, converts into JSON

# Lecture 8: Feeding API Key directly Into Prediction Pipeline
1. 

# Lecture 9: Building Front end of Application
1. Import StreamLit as st
2. st.text_area, st.button, st.write
```
streamlit run app.py
```
# Lecture 10: Testing Front end of Application
1. Can deploy to Streamlit community cloud or custom deployment

# Lecture 11: Writing Docker file
1. Create Dockerfile
```
FROM python:3.9-slim
WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/streamlit/streamlit-example.git .

COPY ./requirements.txt requirements.txt
COPY ./app.py app.py
RUN pip install -r requirements.txt
EXPOSE 8501
ENTRYPOINT ["streamlit", "run"]
CMD ["app.py]
```

# Lecture 12: Packaging and storing in ECR 
1. Set up ECR, build image and push to ECR
2. Console ECR - create new repo - 
```
Login to ECR
docker build -t langchain_streamlit .
Tag the image
Push the image
```

# Lecture 13: Testing packaged application
```
docker run -p 8501:8501 langchain_streamlit:v1
```
2. Could use env variable from command line to pass the AWS secrets to docker

# Lecture 14: Modifying account permissions for deployment
1. ECS with Fargate
2. Cluster - task definition - uses container
3. Create a new role with ECS and Secret Manager
4. Use case ECS - Use case Elastic Container Service Task - create a role
5. Create a new policy and associate with the role

# Lecture 15: Deploying our Apps on ECS
1. Create ECS cluster
2. Create task definition
3. Deploy the task

# Lecture 16: Testing our Apps on ECS
1. 

# Lecture 17: Difference between horizontal vs vertical scaling
1. Load balancer algorithm, round robin

# Lecture 18: Adding App. Load Balancer and Auto Scaling 
1. Create cluster ecs-scalable-cluster with infrastrcuture as AWS Fargate
2. Create new task definition - task definition family as "ecs-scalable-cluster" with Fargate, OS - Linux/ARM64, task role = ecs-task-execution-role-secret, task execution role = ecs-task-execution-role-secret, container - URI of the image in ECR, port = 8501, Environment variable = ARN of the secret manager key, use log collection = 
3. Task definition - select "ecs-scalable-task" - deploy create service - existing cluster "ecs-scalable-cluster" - Compute option Launch type Fargate - Deployment configuration - service name "demo-service" - Deployment type = rolling update - Networking (VPC, Subnets, Network group, Public IP) - Load Balancing: ALB, NLB, None - Create a new load balancer "test-load-balancer"
4. Service auto scaling - min and maximum number of tasks, ECS service metric = ECSServiceAverageCPUUtilization

# Lecture 19: Exposing and testing our service
1. Cluster "ecs-scalable-cluster" - Services "demo-service" - Tasks - networking - ENI Id - Security group - Edit inbound rule
2. Abstract summarization, 
