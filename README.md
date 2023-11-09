
# Movie-API-K8S

This project is a microservices-based application that provides an API for managing movie-related data. It consists of two main components: the api-movies service and a PostgreSQL database. The api-movies service offers RESTful endpoints for CRUD operations on movie data, while the PostgreSQL database stores and manages the persistent data.

## Requirements

* **Google Cloud SDK (gcloud)**:

Required for managing and interacting with Google Cloud resources, including Google Kubernetes Engine (GKE).

You can follow the instructions in the official website: https://cloud.google.com/sdk/docs/install?hl=es-419#linux

* **Docker**:

Used for containerizing the api-movies service for consistent deployment.

You can follow the instructions in the official website: 
https://www.docker.com/get-started/

* **Kubernetes (kubectl)**:

Required to interact with the Kubernetes cluster and apply configuration files.

* **Google Kubernetes Engine (GKE) Cluster**:

The project is designed to run on a Kubernetes cluster, and GKE is used as the target environment.

## Project Installation Guide

### Step 1: Clone the Repository
```bash
# Clone the repository
git clone https://github.com/JulianValencia08/Movie-API-K8S.git
cd Movie-API-K8S
```

### Step 2: Build the Docker Image
```bash
docker build -t api-movies:1.0.0 .
```

### Step 3: Tag the Docker Image
```bash
docker tag api-movies:1.0.0 <DOCKER_USERNAME>/api-movies:1.0.0
```

### Step 4: Log in to Docker Hub
```bash
docker login -u <DOCKER_USERNAME>
```

### Step 5: Push the Docker Image to Docker Hub
```bash
docker push <DOCKER_USERNAME>/api-movies:1.0.0
```

### Step 6: Update Kubernetes YAML Files

Update the **api-movies-deployment.yaml** file to use the newly pushed Docker image. Replace the image field with your Docker Hub username.

```yaml
spec:
    containers:
    - name: api-movies
        image: <DOCKER_USERNAME>/api-movies:1.0.0
        # ... (other configurations)
```

Update the **postgress-config.yaml** file writing your database information.  

```yaml
# ... (other configurations)
data:
  POSTGRES_DB: "<DB_NAME>"
  POSTGRES_USER: "<DB_USER>"
  POSTGRES_PASSWORD: "<DB_PASSWORD>"
```

### Step 7: Deploy to Google Kubernetes Engine (GKE)

```bash
kubectl apply -f k8s-configs/
```

## Results

If you execute this command: 

```bash
kubectl get pods
```

You should see something like this: 

![image](https://github.com/JulianValencia08/Movie-API-K8S/assets/88250984/2f64066d-6261-4520-b579-5bdad79327e0)


Or if you use Open Lens:

![image](https://github.com/JulianValencia08/Movie-API-K8S/assets/88250984/76620f9b-6396-4d28-aff8-246b05a4635c)

* You can download Open Lens in the following link https://github.com/MuhammedKalkan/OpenLens/releases
## Demo

If you make a http request to this POST endpoint: 

```bash
http://<IP>/api/v1.0/films/
```

With the following Json: 

```json
{
    "title": "Pulp Fiction",
    "length": 9180,
    "year": 1994,
    "director": "Quentin Tarantino",
    "actors": [
        { "name": "John Travolta" },
        { "name": "Samuel L. Jackson" },
        { "name": "Uma Thurman" }
    ]
}
```

Now you can go to this GET endpoint: 

```bash
http://<IP>/api/v1.0/films/1
```

You should see something like this: 

![image](https://github.com/JulianValencia08/Movie-API-K8S/assets/88250984/f50eda7d-925d-4273-b0ea-ba30b9843854)



