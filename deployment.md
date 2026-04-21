## 🛠️ Deployment & Configuration Guide

Follow these steps to deploy the detection pipeline and verify it with a simulated attack.

### Phase 1: Infrastructure Prerequisites
Before creating the detection rule, the logging and notification layers must be active.

1. **Configure Amazon SNS (Alerting):**
   * Create a **Standard Topic** (e.g `APICallAlert1`).
   * Create an **Email Subscription** using your email address.
   * **Important:** Check your inbox and click the **Confirm Subscription** link in the email sent by AWS.

2. **Enable AWS CloudTrail:**
   * Ensure you have a **Trail** created in your AWS account.
   * Verify it is configured to log **Management Events** (Read/Write).
   * *Note: EventBridge depends on CloudTrail to "see" the API calls.*

---

### Phase 2: Detection Logic (EventBridge)
1. Navigate to **Amazon EventBridge** > **Rules** > **Customise events**.
2. **Rule Name:** `GetCallerIdentityAlert1`
3. **JSON:** Paste the JSON from [`detection-rules/iam-recon-pattern.json`](detection_rules/iam_recon_pattern.json).
4. **Target:** Select **SNS Topic** and choose the topic created in Phase 1.
5. **Reference:** See [`detection-rules/rule-logic-pix.png`](detection_rules/eventbridge_rule_logic.png) for console configuration.

---

### Phase 3: Attack Simulation (Kali Linux)
Simulate the intruder to test the validity of the rule

1. Open your Kali Linux terminal.
2. Run `aws configure`. 
3. If the AWS CLI is not yet installed, follow the command prompt to install it and rerun the command so we can interact with the AWS console.
4. Ensure AWS CLI is configured with the target access keys.
5. Execute the simulation command: `aws sts get-caller-identity`
