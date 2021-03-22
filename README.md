# Overview

 Using Apache Spark on Cloud Dataproc to distribute a computationally intensive image processing task onto a cluster of machines. Tasks done include:
 - Creating a managed Cloud Dataproc cluster with Apache Spark.
 - Building and running jobs that use external packages not readily available in the cluster.
 - Shutting down the cluster.
 
 
# Introduction

...

# Process

### Step 1:  Create a development machine in Compute Engine

The first step is to create a computing virtual machine. In order to do this you can follow the next steps:
1. Go to Compute Engine > VM Instances > Create
2. Configure the following fields:
   -  Name: devhost
   -  Series: N1
   -  Machine Type: 2 vCPUs (n1-standard-2 instance)
   -  Identity and API Access: Allow full access to all Cloud APIs
 3. Leave the rest of the fields to their default value.
 4. Click Create.
<br>
<br>

### Step 2:  Install external image processing software

Run the following commands in the SSH window of the Compute Engine.
<br>
<br>

**Step 2.1:** Set up Scala and sbt

```
sudo apt-get install -y dirmngr unzip
```
```
sudo apt-get update
```
```
sudo apt-get install -y apt-transport-https
```
```
echo "deb https://dl.bintray.com/sbt/debian /" | \
sudo tee -a /etc/apt/sources.list.d/sbt.list
```
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
```
```
sudo apt-get update
```
```
sudo apt-get install -y bc scala sbt
```
<br>
<br>

**Step 2.2:** Set up Feature Detector Files

```
sudo apt-get update
```
```
gsutil cp gs://spls/gsp124/cloud-dataproc.zip .
unzip cloud-dataproc.zip
```
```
cd cloud-dataproc/codelabs/opencv-haarcascade
```
<br>
<br>

**Step 2.3:** Launch Build

```
sbt assembly
```
<br>
<br>

### Step 3: Create a Cloud Storage bucket and collect images

**Step 3.1:** Name your bucket with Project ID
```
GCP_PROJECT=$(gcloud config get-value core/project)
```
```
MYBUCKET="${USER//google}-image-${RANDOM}"
```
```
echo MYBUCKET=${MYBUCKET}
```
<br>
<br>

**Step 3.2:** Name your bucket with Project ID

### Step 4: Create a Cloud Dataproc cluster

...

### Step 5: Submit the job to Cloud Dataproc

...
