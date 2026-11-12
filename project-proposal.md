# Project Proposal: AI Agent for Optimizing Transaction Processing

Degree level: Bachelor’s

## 1. The problem and goal

**The problem and goal in my project:**

My project focuses on optimizing the performance and memory consumption of a transaction-processing system that supports adding and deleting handlers.

The system consists of two connected components:

- **Transaction Processor:** Receives requests to add or delete handlers. Each operation may trigger expensive processing involving all registered handlers.
- **Transaction Sender:** Contains a main thread responsible for creating transactions and managing the logic, and an I/O thread responsible for sending requests. A queue connects the two threads.

The I/O thread can group multiple requests into batches, allowing the processor to handle several operations together and reduce repeated processing overhead.

However, there are important trade-offs:

- Larger batches may improve throughput but increase latency.
- Larger queues can absorb bursts of requests but allow more transactions to accumulate in memory.
- Excessive memory consumption can degrade performance or lead to out-of-memory failures.
- Small queues and batches may reduce memory usage but cause more blocking and processing overhead.

**The goal is to find the optimal values for three parameters:**

1. The size of the queue between the main thread and the I/O thread.
2. The maximum batch size.
3. The capacity of the queue between the sender and the transaction processor.

The objective is to maximize throughput and minimize latency while keeping memory consumption below a predefined limit and avoiding unbounded accumulation of requests.

Different workloads may have different processing costs, making the optimal configuration workload-dependent.

## 2. The simulator

**The simulator in my project:**

I plan to implement a **C++-based simulator** to achieve high performance and efficiently simulate large workloads. The simulator will model the transaction sender, its main and I/O threads, the queues, and the transaction processor.

The simulator will support different workloads with varying transaction arrival rates, processing times, and request sizes.

It will also track memory consumption caused by queued, batched, and in-flight transactions.

The simulator will accept JSON configuration files specifying the three optimization parameters, workload characteristics, and memory limits.

**The simulator will measure:**

- Throughput (transactions per second).
- Average and 95th-percentile transaction latency.
- Peak memory consumption.
- Maximum and average queue occupancy.
- Processing and validation overhead.
- Number of blocked or rejected transactions caused by full queues.

The simulator will run locally on my computer and support repeated experiments with different configurations, allowing the AI agent to evaluate and compare configurations efficiently.

## 3. The agent's role

**The agent in my project:**

The agent will find the best values for the three parameters while considering performance and memory constraints.

It will repeatedly:

1. Select a configuration for queue sizes and batch size.
2. Run the simulator with representative workloads.
3. Analyze throughput, latency, and memory consumption.
4. Identify bottlenecks, such as excessive queue accumulation or expensive processing operations.
5. Adjust the parameters and repeat the experiment.

For example, increasing batch size might improve throughput, but if memory consumption or request latency becomes too high, the agent should identify the trade-off and consider a smaller batch.

**The primary goal is to maximize throughput subject to latency and memory limits.** Configurations that exceed the memory budget or cause excessive latency will be considered invalid, even if they achieve high throughput.

The language model will help interpret benchmark results, identify bottlenecks, and select promising configurations for subsequent experiments.

I will compare its performance with traditional optimization techniques, such as random search and grid search, to determine whether an AI agent can find good configurations using fewer experiments.

**One open question:** Can an LLM-based agent efficiently find configurations that maximize throughput while respecting strict memory and latency constraints across different workloads?

## Explain your project to someone outside your field

**My project in simple words:**

Imagine a system receiving thousands of requests to add or remove items. Processing each request separately can be expensive, so the system can group requests together and process them as a batch. However, collecting too many requests takes time and consumes memory. If requests arrive faster than they can be processed, they can accumulate and potentially exhaust the system's available memory.

My project aims to find the best balance between speed, response time, and memory usage. I will build a simulator that tests different queue capacities and batch sizes under various workloads. An AI agent will analyze the results and automatically adjust these settings to find configurations that process requests efficiently without allowing excessive memory accumulation.
