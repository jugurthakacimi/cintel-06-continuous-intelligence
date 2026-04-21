# Continuous Intelligence Portfolio

Jugurtha Kacimi

2026-04

This page summarizes my work on **continuous intelligence** projects.

## 1. Professional Project

### Repository Link

[https://github.com/jugurthakacimi/cintel-01-getting-started](https://github.com/jugurthakacimi/cintel-01-getting-started)

### Brief Overview of Project Tools and Choices
In this project, I have made a small modification to the log message in the pipeline to confirm that I can run the project successfully on my machine. The modified line is:

    26: start_time = time.time()
    61: elapsed_time = time.time() - start_time
    62: LOG.info("Total execution time: %.2f seconds", elapsed_time)
This change will allow me to see the execution time in seconds when I run the pipeline, confirming that the project is set up correctly and running on my machine.

## 2. Anomaly Detection

### Repository Link

[https://github.com/jugurthakacimi/cintel-02-static-anomalies](https://github.com/jugurthakacimi/cintel-02-static-anomalies)

### Techniques

* Anomaly Detection - Identify any age or height values that are outside of reasonable limits for adults. This could indicate data entry errors or outliers in the dataset.
* Threshold Used - For age, we will consider values above 95 years or below 21 years as anomalies. For height, we will consider values above 72 inches or below 60 inches as anomalies.

### Artifacts

[https://github.com/jugurthakacimi/cintel-02-static-anomalies/tree/main/artifacts](https://github.com/jugurthakacimi/cintel-02-static-anomalies/tree/main/artifacts)

4 anomalies were found in the dataset based on the defined thresholds.

### Insights

While people aged 96, 102, or 118 could exist, such ages are extremely rare in the general population. For the purposes of my program and learning exercises, these values are flagged as anomalies. Also a person with a height of 73 inches (6 feet 1 inch) is taller than the average adult height, and while not impossible, it is less common, and was added to the anomalies list for this exercise. The goal of this project is to learn how to set thresholds and identify anomalies in static data, so these values are flagged to illustrate the concept of anomaly detection.

## 3. Signal Design

### Repository Link

[https://github.com/jugurthakacimi/cintel-03-signal-design](https://github.com/jugurthakacimi/cintel-03-signal-design)

### Signals

* error_rate: ratio of failed requests to total requests.
* avg_latency_ms: average response time per request.
* throughput: number of requests handled per observation.
* latency_spike: boolean flag that marks observations where average latency exceeded 30ms.

### Artifacts

[https://github.com/jugurthakacimi/cintel-03-signal-design/tree/main/artifacts](https://github.com/jugurthakacimi/cintel-03-signal-design/tree/main/artifacts)

Each observation is now labeled with a True/False spike flag, making it straightforward to filter and isolate high-latency periods without manually scanning numeric columns.

### Insights

The pipeline can now distinguish normal operation from degraded performance automatically.

## 4. Rolling Monitoring

### Repository Link

[https://github.com/jugurthakacimi/cintel-04-rolling-monitoring](https://github.com/jugurthakacimi/cintel-04-rolling-monitoring)

### Techniques

Applied a rolling window of size 3 to smooth short-term fluctuations. Added a normalized error rate to make failure levels comparable across different traffic volumes

### Artifacts

[https://github.com/jugurthakacimi/cintel-04-rolling-monitoring/tree/main/artifacts](https://github.com/jugurthakacimi/cintel-04-rolling-monitoring/tree/main/artifacts)

All signals trend upward over the session. Error rate rises from 1.7% to 4.3%. A dip occurs around 08:35–08:45 as number of requests decreases, then resumes climbing. Rolling means confirm the rise is sustained, not just isolated spikes.

### Insights

The system handles light traffic well but degrades under load. Errors grow faster than requests. Latency follows the same pattern, reinforcing that performance drops as demand increases. Early warning signs appear around 08:20, before traffic peaks.

## 5. Drift Detection

### Repository Link

[https://github.com/jugurthakacimi/cintel-05-drift-detection](https://github.com/jugurthakacimi/cintel-05-drift-detection)

### Techniques

Three difference signals (current avg minus reference avg) with drift flags:

* requests_mean_difference — flagged if absolute difference > 20.0
* errors_mean_difference — flagged if absolute difference > 2.0
* latency_mean_difference_ms — flagged if absolute difference > 1000.0

### Artifacts

[https://github.com/jugurthakacimi/cintel-05-drift-detection/tree/main/artifacts](https://github.com/jugurthakacimi/cintel-05-drift-detection/tree/main/artifacts)

All three drift flags returned False after adding the reference values — no drift detected.

### Insights

Mean-based drift detection self-corrects as accurate data accumulates. A drift flag signals a need to investigate, not necessarily a permanent failure — if values stabilize, the flag clears on its own.

## 6. Continuous Intelligence Pipeline

### Repository Link

[https://github.com/jugurthakacimi/cintel-06-continuous-intelligence](https://github.com/jugurthakacimi/cintel-06-continuous-intelligence)

### Techniques

Pipeline that uses financial transaction data and with thresholds to match the financial domain: flag rate capped at 1% and average transaction amount at $160.

* flag_rate: ratio of flagged to total transactions — detects fraud burst events
* avg_amount_usd: average dollar value per transaction — detects unusual spending patterns

### Artifacts

[https://github.com/jugurthakacimi/cintel-06-continuous-intelligence/tree/main/artifacts](https://github.com/jugurthakacimi/cintel-06-continuous-intelligence/tree/main/artifacts)

The pipeline returned a DEGRADED state. Average flag rate was 1.44%, slightly above the 1% threshold, driven by a handful of days with major fraud spikes (300+ flagged transactions).

### Assessment

The bank's transaction system is under stress from periodic fraud bursts. Even though most days are normal, a few high-fraud days are enough to degrade the overall assessment.
