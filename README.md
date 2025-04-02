# SIT323 5.2D: Deploying a Dockerized Microservice to Google Cloud

This README provides detailed instructions for deploying my Dockerized Node.js microservice to Google Cloud using Artifact Registry. Follow the steps below to build, tag, push, and verify my image in Google Cloud.

---

## Overview

This project is a Dockerized Node.js microservice. In this assignment, you will:
- Enable and configure Google Artifact Registry.
- Build the Docker image locally.
- Tag the image with the correct Artifact Registry path.
- Push the image to Artifact Registry.
- Verify the image upload and pull the image.
- Optionally, run the image to ensure it boots correctly.

**Project Details:**
- **Project ID:** `sit323-25t1-zeyu-lin-c4a5181`
- **Artifact Registry Location:** `us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d`
- **Image Name:** `sit323_5.1p:latest`

---

## Prerequisites

- **Google Cloud SDK** installed on your local machine.
- A **Google Cloud Project** with billing enabled.
- **Artifact Registry API** enabled in your project.
- **Docker** installed and running.
- Basic knowledge of Docker commands and Google Cloud operations.

---

## Step-by-Step Instructions

### 1. Enable Artifact Registry API

Before using Artifact Registry, ensure that the API is enabled in your project. Run the following command:

```bash
gcloud services enable artifactregistry.googleapis.com
```

Alternatively, you can enable it via the [Google Cloud Console](https://console.cloud.google.com/).

---

### 2. Configure Docker Authentication

Configure Docker to authenticate with Artifact Registry using gcloud:

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

This command updates your `~/.docker/config.json` to use gcloud as the credential helper for Artifact Registry.

---

### 3. Build the Docker Image

Navigate to your project directory (where the `Dockerfile` is located) and build the image:

```bash
docker build -t sit323_5.1p:latest .
```

Verify that the image has been built by listing local Docker images:

```bash
docker images
```

---

### 4. Tag the Docker Image

Tag your image with the Artifact Registry repository path. Run:

```bash
docker tag sit323_5.1p:latest us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d/sit323_5.1p:latest
```

This command associates your local image with the remote repository location in Artifact Registry.

---

### 5. Push the Docker Image to Artifact Registry

Push the tagged image to Artifact Registry:

```bash
docker push us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d/sit323_5.1p:latest
```

Monitor the output to ensure that all layers are uploaded successfully. If you encounter authentication or permission errors, verify that you have the correct IAM roles (such as `roles/artifactregistry.writer` or `roles/storage.admin`) and that your gcloud credentials are up to date.

---

### 6. Verify the Image Upload

To confirm that the image has been uploaded, list the images in your Artifact Registry repository:

```bash
gcloud artifacts docker images list us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d
```

Alternatively, check the Artifact Registry section in the Google Cloud Console.

---

### 7. Pull the Image from Artifact Registry

To test that your image can be pulled from Artifact Registry on any machine, run:

```bash
docker pull us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d/sit323_5.1p:latest
```

This ensures that the image is available for deployment in other environments.

---

### 8. Run the Container Locally

(Optional) Run the container locally to verify that it boots correctly:

```bash
docker run -d -p 8080:8080 us-central1-docker.pkg.dev/sit323-25t1-zeyu-lin-c4a5181/sit323-5d/sit323_5.1p:latest
```

Open your browser and navigate to `http://localhost:8080` (or the appropriate port) to test the service.

---

## Troubleshooting

- **Authentication Issues:**  
  - Ensure you have run `gcloud auth login` and `gcloud auth configure-docker us-central1-docker.pkg.dev`.
  - If issues persist, try logging in manually:
    ```bash
    gcloud auth print-access-token | docker login -u oauth2accesstoken --password-stdin https://us-central1-docker.pkg.dev
    ```

- **Permission Errors (403 Forbidden):**  
  - Check that your account has the required IAM roles (e.g., `roles/artifactregistry.writer` or `roles/storage.admin`).
  - Verify that your project is set correctly using:
    ```bash
    gcloud config set project sit323-25t1-zeyu-lin-c4a5181
    ```

- **Image Tagging Issues:**  
  - Ensure the tag follows the format:  
    `us-central1-docker.pkg.dev/<PROJECT_ID>/<REPOSITORY>/<IMAGE>:<TAG>`
