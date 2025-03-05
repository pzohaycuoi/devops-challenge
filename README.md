# DevOps Challenge

This project is a solution to a DevOps challenge designed to test various skills in system administration, cloud architecture, and troubleshooting. The project is divided into three main problems, each focusing on different aspects of DevOps.

## Table of Contents

- [Problem 1: Too Many Things To Do](#problem-1-too-many-things-to-do)
- [Problem 2: Building Castle In The Cloud](#problem-2-building-castle-in-the-cloud)
- [Problem 3: Diagnose Me Doctor](#problem-3-diagnose-me-doctor)

## Problem 1: Too Many Things To Do

### Task

Given a transaction log file, write a CLI command to filter and process the data.

#### Objective

Submit an HTTP GET request to `https://example.com/api/:order_id` with Order IDs that are selling `TSLA`, writing the output to `./output.txt`.

#### Solution

1. **`jq` Filtering**: Uses `jq` to read JSON objects from `./transaction-log.txt`. The command filters out those objects where the symbol is `"TSLA"` and the side is `"sell"`, then extracts the `order_id` field from each matching object.
2. **Using `xargs` and `curl`**: For each `order_id` output by `jq`, `xargs` replaces the placeholder `{}` in the curl URL. It sends an HTTP GET request to `https://example.com/api/<order_id>` for each order ID in a silent mode (i.e., not showing progress or error messages).
3. **Appending Output**: The responses from the curl requests are appended to `./output.txt`.

### Files

- `transaction-log.txt`: Contains the transaction log data.
- `output.txt`: The file where the output of the curl requests is stored.
- `oderfilterrequest.sh`: The script that contains the CLI command to perform the task.

## Problem 2: Building Castle In The Cloud

### Solution Overview

This solution is designed as a highly available, low-latency trading system inspired by platforms such as Binance. It operates in an **active-active** configuration across two AWS Regions, ensuring minimal downtime and rapid failover in the event of a regional disruption.

### Key Sections

1. **Architecture at a Glance**
2. **Data Flow**
3. **High Availability Strategy**
4. **Performance and Low Latency Considerations**
5. **Scalability Approach**
6. **Security and Compliance**
   - Network Segregation
   - Encryption (TLS, KMS)
   - Secrets Management
   - AWS WAF & Security Groups
   - Monitoring and Audit (CloudTrail, CloudWatch, X-Ray)
7. **Deployment and CI/CD**
   - AWS CodePipeline
   - AWS CodeBuild
   - AWS CodeDeploy
   - Infrastructure as Code (IaC)
8. **Future Growth and Evolution**

### Files

- `problem2.drawio`: Diagram of the architecture.
- `imgs/architect.png`: Image of the architecture diagram.

## Problem 3: Diagnose Me Doctor

### Troubleshooting for High Memory Usage on NGINX VM

This section provides a step-by-step guide to troubleshoot high memory usage on an NGINX VM.

### Key Sections

1. **Verify Memory Usage via CLI**
2. **Identify Processes Consuming Memory**
3. **Check NGINX Configuration**
4. **Check for Memory Leaks**
5. **Investigate Non-NGINX Processes**
6. **Check Kernel and System Health**
7. **Possible Root Causes, Impacts, and Recovery**

### Files

- `README.md`: Contains the detailed troubleshooting steps.

## Conclusion

This project demonstrates the ability to handle various DevOps tasks, including data processing with CLI tools, designing a highly available cloud architecture, and troubleshooting system issues. Each problem is documented with detailed steps and solutions to guide the reader through the process.

---

For any questions or further clarifications, please feel free to reach out.