# Lab M2.06 - CloudWatch Monitoring Report

## 1. Step-by-Step Implementation
1.  **Notification**: Created SNS Topic `ec2-alerts` and confirmed email subscription.
2.  **Alarm**: Configured CloudWatch Alarm `high-cpu-alarm` (Threshold: >80% CPU for 10 minutes/2 periods).
3.  **Stress Test**: SSH into EC2 and ran `stress --cpu 2 --timeout 600s`.
4.  **Verification**: Confirmed metric spike, Alarm state change to `ALARM`, and email delivery.

## 2. Importance of Monitoring
Monitoring is essential for **system reliability**. It allows for:
*   **Proactive Issue Detection**: Catching performance degradation before it affects users.
*   **Automated Response**: Triggering actions like Auto Scaling to maintain availability.

## 3. Alarm Threshold Logic
*   **Threshold (80%)**: Selected as a critical warning level to allow time for intervention before 100% saturation.
*   **Evaluation Periods (2 of 2)**: Prevents false alarms from short, non-critical CPU spikes.

## 4. SNS Workflow
1.  **Trigger**: CloudWatch detects threshold breach.
2.  **Publish**: CloudWatch sends an event to the SNS Topic.
3.  **Deliver**: SNS pushes the alert to the subscribed email endpoint.

## 5. Stress Test Observations
*   **Metrics**: CPU spiked to 100% immediately, but the alarm had a ~10min lag due to the 5-minute aggregation window.
*   **Recovery**: Once `stress` ended, CPU normalized and the alarm returned to `OK` status automatically.

## 6. Real-World Use Cases
*   **Auto Scaling**: Adding instances when CPU is consistently high.
*   **Billing**: Alerting when monthly spend exceeds a set budget.
*   **Health Checks**: Restarting failed instances when `StatusCheckFailed` is triggered.