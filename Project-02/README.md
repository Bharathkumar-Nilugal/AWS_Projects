# AWS CloudTrail Setup – Step-by-Step Guide

This repository contains a concise PDF guide documenting the setup of AWS CloudTrail for logging and monitoring AWS account activity. It includes the core configuration steps along with supporting screenshots.

---

## Steps Performed

1. **Created a new CloudTrail trail**  
   - Trail name: `Task02-demo-trail`

2. **Enabled multi-region logging**  
   - Captures events across all AWS regions

3. **Enabled log file validation**  
   - Ensures integrity of each log file

4. **Configured storage destinations**  
   - **Amazon S3**: Stores the logs securely  
   - **CloudWatch Logs**: Allows monitoring and alerting

5. **Enabled management event logging**  
   - Captures API activity (e.g., for KMS, RDS Data API)

6. **Verified trail setup**  
   - Confirmed trail is active and logging in CloudTrail console

7. **Checked log delivery**  
   - Verified that `.json.gz` log files are being delivered to the S3 bucket

8. **Captured screenshots**  
   - Screenshots included in the guide for reference

---

##  Documentation

**[Download the PDF Guide](./AWS_CloudTrail.pdf)**  
Includes all the steps and screenshots as visual proof of configuration.

---

## 📁 Repository Contents

| File Name                               | Description                        |
|----------------------------------------|------------------------------------|
| `AWS_CloudTrail.pdf`                   | Final guide with steps + screenshots |
| `README.md`                            | This overview file                 |

---

## Use Case

This setup is ideal for:

- Audit and compliance logging  
- Monitoring security-related API activity  
- Investigating incidents in your AWS environment

---

## Notes

- Trail is **not** configured for organization-wide use or insights (in this guide).
- The configuration is done in the **Asia Pacific (Mumbai)** region but applies across regions.
- Log validation and multi-region logging are **enabled** for better security.

---

## License

Documentation is free to use and modify for personal or professional use.
