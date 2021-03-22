# Overview

 Using Apache Spark on Cloud Dataproc to distribute a computationally intensive image processing task onto a cluster of machines. Tasks done include:
 - Creating a managed Cloud Dataproc cluster with Apache Spark.
 - Building and running jobs that use external packages not readily available in the cluster.
 - Shutting down the cluster.
<br>


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

**Step 3.2:** Create the bucket
```
gsutil mb gs://${MYBUCKET}
```
<br>
<br>

**Step 3.3:** Download images to the bucket
```
curl https://www.publicdomainpictures.net/pictures/20000/velka/family-of-three-871290963799xUk.jpg | gsutil cp - gs://${MYBUCKET}/imgs/family-of-three.jpg
```
```
curl https://www.publicdomainpictures.net/pictures/10000/velka/african-woman-331287912508yqXc.jpg | gsutil cp - gs://${MYBUCKET}/imgs/african-woman.jpg
```
```
curl https://www.publicdomainpictures.net/pictures/10000/velka/296-1246658839vCW7.jpg | gsutil cp - gs://${MYBUCKET}/imgs/classroom.jpg
```
<br>
<br>

**Step 3.4:** Check contents of the bucket
```
gsutil ls -R gs://${MYBUCKET}
```
<br>
<br>

### Step 4: Create a Cloud Dataproc cluster

**Step 4.1:** Check contents of the bucket
```
MYCLUSTER="${USER/_/-}-qwiklab"
```
```
echo MYCLUSTER=${MYCLUSTER}
```
<br>
<br>

**Step 4.2:** Check contents of the bucket
```
gcloud config set dataproc/region us-central1
```
```
gcloud dataproc clusters create ${MYCLUSTER} \
--bucket=${MYBUCKET} \
--worker-machine-type=n1-standard-2 \
--master-machine-type=n1-standard-2 \
--initialization-actions=gs://spls/gsp010/install-libgtk.sh \
--image-version=2.0  
```


### Step 5: Submit the job to Cloud Dataproc

**Step 5.1:** Load the face detection configuration file
```
curl https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_default.xml | gsutil cp - gs://${MYBUCKET}/haarcascade_frontalface_default.xml
```
<br>
<br>

**Step 5.2:** Input images to the face detector and submit file
```
cd ~/cloud-dataproc/codelabs/opencv-haarcascade
```
```
gcloud dataproc jobs submit spark \
--cluster ${MYCLUSTER} \
--jar target/scala-2.12/feature_detector-assembly-1.0.jar -- \
gs://${MYBUCKET}/haarcascade_frontalface_default.xml \
gs://${MYBUCKET}/imgs/ \
gs://${MYBUCKET}/out/
```
<br>
<br>

### Step 6: Check Output

**Step 6.1:** Monitor the job
Go to  Navigation menu > Dataproc > Jobs. Check that Status says Succeeded.

**Step 6.2:** Monitor the job
Go to Navigation menu > Sorage. You can find the bucket you created and retrieve the images and the output.
<br>
<br>

Reference Qwiklab: [Distributed Image Processing in Cloud Dataproc](https://www.qwiklabs.com/focuses/5834?catalog_rank=%7B%22rank%22%3A7%2C%22num_filters%22%3A0%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=4914974)
