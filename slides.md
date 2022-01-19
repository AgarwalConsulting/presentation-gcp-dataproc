layout: true

.signature[@algogrit]

---

class: center, middle

# GCP DataProc

Gaurav Agarwal

---
class: center, middle

Dataproc is a managed Spark and Hadoop service that lets you take advantage of open source data tools for batch processing, querying, streaming, and machine learning.

---
class: center, middle

Dataproc automation helps you create clusters quickly, manage them easily, and save money by turning clusters off when you don't need them.

---

## Why use Dataproc?

- Low cost

- Super fast

- Integrated

- Managed

- Simple and familiar

---
class: center, middle

## What is included in DataProc?

---
class: center, middle

When you create a cluster, standard Apache Hadoop ecosystem components are automatically installed on the cluster.

---
class: center, middle

[List](https://cloud.google.com/dataproc/docs/concepts/versioning/dataproc-release-2.0) of open source (Hadoop, Spark, Hive, and Pig) and Google Cloud Platform connector versions supported

---
class: center, middle

You can install additional components, called "optional components" on the cluster when you create the cluster.

---
class: center, middle

![Optional Components](assets/images/optional-components.png)

.image-credits[https://cloud.google.com/dataproc/docs/concepts/components/overview]

---
class: center, middle

## Managing clusters

---
class: center, middle

`gcloud dataproc clusters ...`

---
class: center, middle

### Cluster configuration

---

- HA, Single Master, Single Node

- Master/Worker Types

- Optional Components

- Image

---
class: center, middle

Listing the nodes

```bash
gcloud compute instances list
```

---
class: center, middle

*Demo*: Accessing hadoop and spark within the cluster nodes

---
class: center, middle

## Creating a image

.content-credits[https://cloud.google.com/dataproc/docs/guides/dataproc-images]

---
class: center, middle

## Submitting jobs

---
class: center, middle

*Demo*: Scheduling our first job

---
class: center, middle

### Life of a Dataproc Job

.content-credits[https://cloud.google.com/dataproc/docs/concepts/jobs/life-of-a-job]

---

`1.` User submits job to Dataproc.

  - JobStatus.State is marked as `PENDING`.

`2.` Job waits to be acquired by the `dataproc` agent.

  - If the job is acquired, JobStatus.State is marked as `RUNNING`.

  - If the job is not acquired due to agent failure, Compute Engine network failure, or other cause, the job is marked `ERROR`.

`3.` Once a job is acquired by the agent, the agent verifies that there are sufficient resources available on the Dataproc cluster's master node to start the driver.

  - If sufficient resources are not available, the job is delayed (throttled). JobStatus.Substate shows the job as `QUEUED`, and Job.JobStatus.details provides information on the cause of the delay.

  - If sufficient resources are available, the `dataproc` agent starts the job driver process.

---

`4.` At this stage, typically there are one or more applications running in Apache Hadoop YARN. However, Yarn applications may not start until the driver finishes scanning Cloud Storage directories or performing other start-up job tasks.

`5.` The `dataproc` agent periodically sends updates to Dataproc on job progress, cluster metrics, and Yarn applications associated with the job.

`6.` Yarn application(s) complete.

  - Job continues to be reported as `RUNNING` while driver performs any job completion tasks, such as materializing collections.

  - An unhandled or uncaught failure in the Main thread can leave the driver in a zombie state (marked as `RUNNING` without information as to the cause of the failure).

`7.` Driver exits. `dataproc` agent reports completion to Dataproc.

  - Dataproc reports job as `DONE`.

---
class: center, middle

### Job concurrency

---
class: center, middle

Formula

`max((masterMemoryMb - 3584) / masterMemoryMbPerJob, 5)`

---
class: center, middle

`masterMemoryMbPerJob` is 1024 by default

---

- `--max-concurrent-jobs`

- `--driver-size-mb`

---
class: center, middle

Code
https://github.com/algogrit/presentation-gcp-dataproc

Slides
https://gcp-dataproc.slides.agarwalconsulting.io
