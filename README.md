# Overview

 Using Apache Spark on Cloud Dataproc to distribute a computationally intensive image processing task onto a cluster of machines. Tasks done include:
 - Creating a managed Cloud Dataproc cluster with Apache Spark.
 - Building and running jobs that use external packages not readily available in the cluster.
 - Shutting down the cluster.
 
 
# Introduction

...

# Process

### Step 1:  Create a development machine in Compute Engine




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

### Step 3: Create a Cloud Storage bucket and collect images

...

### Step 4: Create a Cloud Dataproc cluster

...

### Step 5: Submit the job to Cloud Dataproc

...
