# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)


**Task 1:**
Components:
Operational data: data/service_data.json
Metrics and logs: The operational data contains response time, CPU, memory, log level and log message
Anomaly detection: src/anomaly_detector.py
Event production: src/event_producer.py
Event topic: src/event_topic.py
Event consumption: src/event_consumer.py
AIOps processing: src/aiops_pipeline.py
Validation: tests/test_aiops_pipeline.py

Service Being Monitored:
The monitored service is payment-service

Operational Problem:
The main problem is identifying unusual behaviour in the payment service. The data contains normal records as well as records with high response time, CPU and memory usage and error logs.

Purpose of AIOps:
The purpose of AIOps in this assessment is to detect abnormal service behaviour and move the detected anomaly through the event-processing pipeline

**Task 2:**

Metrics: response_time_ms, cpu_percent, and memory_percent.
Log fields:  log_level and message. service identifies the monitored service.
Timestamps: Records are taken every minute from 10:00 to 10:09
Normal behaviour: Most observations from 10:00 10:04 and 10:07 10:09 are normal, with low response time, moderate CPU/memory usage, and imfo logs.
Unusual behaviour: 10:05 and 10:06 stand out  Response time rises to 610–640 ms, CPU to 75–94%, and memory to 70–91%. Both have ERROR logs for timeouts.

thererfore, the main abnormal period is 10:05–10:06.

**Task 3:**
The anomaly detector processed the operational data and detected anomalies at 10:05 and 10:06.

At 10:05, the response time was 610 ms and CPU and memory usage were also higher than normal. same at 10:06

the detector currently checks for WARNING logs instead of ERROR logs thats why ERROR logs events are not being identified
(        if record["log_level"] == "WARNING": <----- this
            reasons.append("Error log detected")

        if not reasons:
            return None)

No normal event was incorrectly flagged.

One limitation is that the detector uses fixed thresholds, so some gradual or less obvious changes might not be detected.

**Task 4:**
when i run the aiops_pipeline.py i get 
AIOps Pipeline Result
==================================================
Records processed: 10
Anomalies detected: 2
Events consumed: 0

-Anomaly identified: yes the anomaly detector identified 2 anomalies from the 10 records.
-Event passed to Producer: Yes. When an anomaly is detected, the pipeline passes the generated event to Producer 
(producer.publish(event)) <------ this
-Producer publishes to Topic: Yes the producer publishes the anomaly events to the service topic.
-Consumer receives the event: No. The consumer received 0 events.
-Consumer processes the received event: No Since the consumer received 0 events so no events to process
Event reaches downstream AIOps component: No Because the event did not reach the consumer
roles of the following:
producer: Publishes detected anomaly events.
Topic: Carries events between the producer and consumer.
Consumer: Receives and processes events from the topic.
Event: Contains the detected anomaly information such as timestamp, service, type, and reasons.

**Task 5:**

1.Anomaly_Detector.py:
Component: AnomalyDetector
Problem: The detector checked for WARNING logs, but the data contained ERROR logs
Correction: Changed the condition from WARNING to ERROR
Verification: ERROR events were correctly identified.

2.Event Producer/Consumer
Component: AIOps_pipeline.py
Problem: The producer and consumer were using different topics.
Correction: Changed the consumer to use the same topic as the producer.
Verification: Events consumed increased from 0 to 2.

Final result:
@yuvrajsinghrathoree2024 ➜ /workspaces/AiOps_MSE1 (main) $ PYTHONPATH=src python src/aiops_pipeline.py
==================================================
AIOps Pipeline Result
==================================================
Records processed: 10
Anomalies detected: 2
Events consumed: 2

Detected Events:

Service: payment-service
Timestamp: 2026-09-20T10:05:00
Type: ANOMALY
Reasons: High response time, Error log detected

Service: payment-service
Timestamp: 2026-09-20T10:06:00
Type: ANOMALY
Reasons: High response time, High CPU utilization, High memory utilization, Error log detected

**Task 6:** 

The complete AIOps pipeline was executed.

1. 10 operational records were processed.
2. 2 anomalies were detected at 10:05 and 10:06.
3. Anomaly events were generated.
4. The producer published the events.
5. Both events were consumed successfully.
6. The consumer processed and displayed the events.
7. The final output showed the operational issues, including high response time, high CPU usage, and error logs.

Final result:
==================================================
AIOps Pipeline Result
==================================================
Records processed: 10
Anomalies detected: 2
Events consumed: 2

Detected Events:

Service: payment-service
Timestamp: 2026-09-20T10:05:00
Type: ANOMALY
Reasons: High response time, Error log detected

Service: payment-service
Timestamp: 2026-09-20T10:06:00
Type: ANOMALY
Reasons: High response time, High CPU utilization, High memory utilization, Error log detected

**Task 7:**

1.AIOps Scenario
This project monitors a payment service using logs and system metrics. The aim is to find unusual behaviour and send the detected problems

2.Operational Data
The data contains payment service records from 10:00 to 10:09
The main metrics are:
Response time
CPU usage
Memory usage
It also contains the log level and message for each record

3.Observations
Most records were normal from 10:00 to 10:04 and 10:07 to 10:09
At 10:05 and 10:06 the values changed a lot. Response time increased to 610 ms and 640 ms. CPU and memory usage also increased. Both records had ERROR logs

4.Anomaly Detection
The detector found 2 anomalies at 10:05 and 10:06.
The 10:05 record had high response time and an ERROR log.
The 10:06 record had high response time, high CPU, high memory and an ERROR log

5.Event Flow
The flow is:
Operational Data → Anomaly Detection → Event → Producer → Topic → Consumer → AIOps
The detector creates an event when an anomaly is found The producer sends it to the topic and the consumer receives and processes it

6.Final Result
After fixing the issues, the pipeline gave:
Records processed: 10
Anomalies detected: 2
Events consumed: 2

The events for 10:05 and 10:06 were successfully received by the consumer.

7.Issues Found and Fixed
I found two issues
1.The detector was checking for WARNING logs, but the data had ERROR logs. I changed it to check for ERROR.
2.The producer and consumer were using different topics. I changed the consumer to use the same topic as the producer.

8.Limitation

The detector uses fixed thresholds. It may not detect some unusual behaviour if the values do not cross the fixed limits.
A possible improvement would be to use historical data to set the thresholds automatically.

9.How to Run

1.Open the repository in Codespaces.
2.Run the pipeline:

on terminal
PYTHONPATH=src python src/aiops_pipeline.py

Task 8:
python pytest result
plugins: cov-7.1.0
collected 8 items                                                                                 

tests/calculations_test.py::test_area_of_circle_positive_radius PASSED                      [ 12%]
tests/calculations_test.py::test_area_of_circle_zero_radius PASSED                          [ 25%]
tests/calculations_test.py::test_get_nth_fibonacci_zero PASSED                              [ 37%]
tests/calculations_test.py::test_get_nth_fibonacci_one PASSED                               [ 50%]
tests/test_aiops_pipeline.py::test_normal_record_is_not_anomaly PASSED                      [ 62%]
tests/test_aiops_pipeline.py::test_anomalous_record_is_detected PASSED                      [ 75%]
tests/test_aiops_pipeline.py::test_producer_publishes_event PASSED                          [ 87%]
tests/test_aiops_pipeline.py::test_consumer_receives_event PASSED                           [100%]

======================================== 8 passed in 0.06s ========================================

python src/aiops_pipeline.py result
Events consumed: 2

Detected Events:

Service: payment-service
Timestamp: 2026-09-20T10:05:00
Type: ANOMALY
Reasons: High response time, Error log detected

Service: payment-service
Timestamp: 2026-09-20T10:06:00
Type: ANOMALY
Reasons: High response time, High CPU utilization, High memory utilization, Error log detected

everything is running perfectly