# Large Scale Data Processing: Project 1 Report - Bitcoin Mining with Spark

This project involves developing and executing a program to find a nonce that, when hashed with a given input string, results in a hash that meets a certain difficulty level. The difficulty is defined by the number of leading zeros in the hash output. This report summarizes the findings of running the program with various difficulty levels on both a local machine and Google Cloud Platform (GCP).


## Local Machine Results

The program was run on a local machine with different difficulty levels (`k`) and number of trials (`n`). Below are the results for each run:

| Difficulty (`k`) | Number of Trials (`n`) | Nonce (`xS`)         | Hash Value                                                        | Time Elapsed (s) |
|------------------|------------------------|----------------------|-------------------------------------------------------------------|------------------|
| 2                | 100                    | 772884102            | 00307de3318c1f4b7a96d79516b94658bb086a98a1d906f733afe0ed6b7cc80f | 1                |
| 3                | 10,000                 | 384129572            | 000adcf5a6877744faebfa88ff51bb42aa7aa57789135999869efc5d9912222f | 1                |
| 4                | 100,000                | 1749980439           | 00001ab43ec613c65ff2e9235ef2b15691c7bf3052c60239e4adaf23ac2055d2 | 1                |
| 5                | 1,000,000              | 1898532701           | 00000ab037336a5b2f0dbc6480b88e653db4f32981fd7f2691140337cc48e762 | 2                |
| 6                | 10,000,000             | 63370624             | 000000fa1a20128cf92d1e54dd4ea19246272c448dcfda8c3df87d49a7a23be4 | 4                |


## GCP Results

For difficulty level `k = 7`, the program was run on Google Cloud Platform due to the increased computational requirements. The configuration of the cluster and the process for estimating the number of trials are discussed below.

### Case: `k = 7`

- **Number of Trials (`n`):** 100,000,000
- **Nonce (`xS`):** 1017559068
- **Hash Value:** 0000000e78c7f2126bd186832483c35f2782e84ecd3845b17fe7b0d546729400
- **Time Elapsed:** 347s

### Cluster Configuration

The GCP cluster used to solve the case `k = 7` had the following configuration:

- **Region:** us-central1
- **Zone:** us-central1-f
- **Master Node Machine Type:** n2-standard-4 (4 vCPUs, 16 GB memory)
- **Primary Disk Type:** pd-balanced
- **Primary Disk Size:** 500GB
- **Google Cloud Storage Staging Bucket:** Provided
- **Image Version:** 2.1.42-debian11
- **Network:** Default
- **Created On:** March 12, 2024, at 3:22:35 PM
- **Encryption Type:** Google-managed

This configuration indicates that the cluster consisted of a single master node without additional worker nodes.

### Estimating the Number of Trials

Based on initial tests and performance benchmarks conducted on a local machine across varying difficulty levels, we determined that an n2-standard-4 master node could feasibly compute 100,000,000 trials to find a suitable nonce for k = 7. This decision was informed by the node’s computational capacity and the extrapolated difficulty curve from previous local runs.


## Efficiency Discussion: Deterministic vs. Randomized Nonce Generation

*The deterministic approach* is more efficient in terms of ensuring complete coverage of the search space without duplication of effort. It's predictable and systematically exhaustive. This makes it preferable for problems where the solution is expected to be found within a manageable number of tries or when running the process in a tightly controlled environment.

*The randomized approach* could be more "efficient" in practice for distributed systems or very large search spaces because it allows different nodes or instances to explore the search space independently without coordination, reducing the chance of overlap. However, its efficiency is subject to variance and luck due to its random nature.

For a controlled environment with a relatively small search space, the deterministic method is more efficient and straightforward. In contrast, for large, distributed systems with vast search spaces, the randomized method may offer practical advantages.


## Contributors
Peiyu Zhong, Kelsey Zeng