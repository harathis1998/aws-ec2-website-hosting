# AWS EC2 Website Hosting & CloudWatch Monitoring

## Project Overview

A hands-on AWS learning project focused on hosting a static website on an Amazon EC2 Linux instance, monitoring CPU utilization with Amazon CloudWatch, and configuring alarm notifications through Amazon SNS.

The project demonstrates foundational AWS infrastructure, Linux administration, monitoring, troubleshooting, and cost-awareness skills relevant to AWS Cloud Support and Cloud Operations roles.

## Objectives

- Launch and configure an Amazon EC2 Linux instance.
- Configure SSH access using an EC2 key pair.
- Control network access using security groups.
- Install and configure the Apache HTTP Server.
- Deploy and verify a static website.
- Explore Amazon S3 for object storage practice.
- Monitor EC2 CPU utilization using Amazon CloudWatch.
- Create a CPU utilization alarm.
- Configure Amazon SNS email notifications.
- Practise basic troubleshooting and cost management.

## AWS Services and Technologies

- Amazon EC2
- Amazon CloudWatch
- Amazon SNS
- Amazon S3 — storage practice
- Amazon Linux 2023
- Apache HTTP Server
- SSH
- EC2 Key Pairs
- AWS Security Groups

## Architecture

**Website request flow**

User → Internet → EC2 Security Group → EC2 Instance → Apache Web Server → Static Website

**Monitoring and alerting flow**

EC2 Instance → CloudWatch CPU Metrics → CloudWatch Alarm → Amazon SNS → Email Notification

## Implementation Steps

### 1. Launch an EC2 Instance

- Selected Amazon Linux 2023 as the operating system.
- Launched a `t3.micro` instance.
- Configured a security group for SSH and HTTP access.
- Connected to the instance using SSH from Windows PowerShell.

### 2. Install and Configure Apache

Installed Apache and enabled the service:

```bash
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```

Created a simple HTML page:

```bash
echo '<h1>AWS Monitoring Lab</h1><p>My first website hosted on Amazon EC2!</p>' | sudo tee /var/www/html/index.html
```

### 3. Verify Website Availability

Tested the web server using:

```bash
curl -I http://localhost
```

**Observed result:** `HTTP/1.1 200 OK`

This confirmed that Apache was responding successfully to HTTP requests from the EC2 instance.

### 4. Monitor CPU Utilization

Used Amazon CloudWatch to view the EC2 `CPUUtilization` metric.

- Namespace: `AWS/EC2`
- Metric: `CPUUtilization`
- Statistic: Average
- Period: 5 minutes

### 5. Configure a CloudWatch Alarm

Created an alarm named `aws-monitoring-lab-high-cpu`.

| Setting | Value |
|---|---|
| Metric | `CPUUtilization` |
| Threshold | Greater than 70% |
| Period | 5 minutes |
| Datapoints to alarm | 1 out of 1 |
| Missing data treatment | Treat as missing |

The alarm is configured to enter the `ALARM` state when the CPU utilization metric breaches the configured threshold.

### 6. Configure Amazon SNS Notifications

- Created an SNS topic named `aws-monitoring-lab-alerts`.
- Configured the CloudWatch alarm to publish notifications to this topic.
- Confirmed the email subscription.

**Validation status:** The SNS email subscription is confirmed. Delivery of an actual alarm notification email has not yet been verified.

### 7. Manage AWS Costs

- Configured an AWS monthly budget with alerts.
- Reviewed the AWS Credits and Bills pages.
- Stopped the EC2 instance when it was not needed for practice.

Stopping an EC2 instance prevents ongoing instance compute charges, but EBS storage and other resources may continue to incur charges.

## Validation Results

- [x] EC2 instance launched.
- [x] SSH connection established.
- [x] Apache web server installed and running.
- [x] Website returned HTTP 200.
- [x] EC2 CPU metrics visible in CloudWatch.
- [x] CloudWatch CPU alarm created.
- [x] SNS email subscription confirmed.
- [x] Alarm observed in `OK` state.
- [x] EC2 instance stopped after practice.
- [ ] Actual transition to the `ALARM` state verified.
- [ ] Actual alarm notification email received and verified.
- [ ] Amazon S3 practice documented and validated.

## Troubleshooting Experience

### SSH Private Key Permissions

**Issue:** SSH rejected the private key because its Windows file permissions were too open.

**Resolution:** Restricted inherited permissions, granted the current Windows user read access, and removed the additional account identified in the SSH error.

### Website Verification

**Test:** Executed `curl -I http://localhost`.

**Result:** Received `HTTP/1.1 200 OK`, confirming that the Apache web server responded successfully.

### CloudWatch Alarm State

**Observation:** The alarm initially displayed `Insufficient data` and later transitioned to `OK`.

**Learning:** CloudWatch requires sufficient metric data to evaluate the configured alarm condition. An `OK` state means the alarm's threshold is not currently breached.

## Security Considerations

- Restricted SSH access to the permitted source IP address.
- Allowed HTTP access for the public website.
- Kept the EC2 private key outside the Git repository.
- Avoided publishing AWS credentials, account identifiers, private keys, or sensitive screenshots.
- Reviewed billing information and stopped the instance when it was not needed.

## Skills Demonstrated

- EC2 provisioning and configuration
- Linux command-line fundamentals
- Apache installation and configuration
- SSH troubleshooting
- Security group configuration
- CloudWatch metrics and alarms
- Amazon SNS notification configuration
- AWS cost awareness
- Basic troubleshooting documentation

## Future Improvements

- Verify end-to-end alarm email delivery.
- Safely test a CloudWatch alarm state transition.
- Complete and document the Amazon S3 storage exercise.
- Add a website troubleshooting runbook.
- Explore Terraform for infrastructure automation.

## Project Status

Personal hands-on AWS learning project. The website deployment, CloudWatch alarm configuration, and SNS subscription setup are complete. End-to-end alarm notification testing remains pending.

This project is intended for learning and portfolio demonstration, not production use.
