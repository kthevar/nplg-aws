### Architecture Overview

The PLNG Stack architecture is designed to provide a comprehensive solution for log management and visualization, leveraging several key AWS services and open-source tools.

#### 1. **Amazon EC2 Instance (PLGInstance):**
   - An Amazon Linux 2 instance acts as the host for the entire PLNG Stack.
   - It is configured with a t2.micro instance type, providing a baseline level of CPU performance.

#### 2. **Security Group (PLGSecurityGroup):**
   - The PLGSecurityGroup controls incoming traffic to the EC2 instance.
   - It allows secure communication on ports 1443, 443, and 3000, facilitating access to Loki and Grafana.

#### 3. **Amazon S3 Bucket (LokiBucket):**
   - The LokiBucket serves as the backend storage for Loki logs.
   - It provides a scalable and durable solution for storing log data.

#### 4. **IAM Role and Instance Profile (InstanceRole, InstanceProfile):**
   - The InstanceRole defines the permissions needed by the EC2 instance.
   - The InstanceProfile associates the role with the EC2 instance, allowing it to interact with other AWS services.

#### 5. **Loki (Log Aggregator):**
   - Loki is a horizontally scalable log aggregation system inspired by Prometheus.
   - It is configured to store logs in the S3 bucket, providing efficient and cost-effective log storage.
   - The configuration includes schema settings, storage configurations, and compaction intervals.

#### 6. **NGINX (Web Server and Reverse Proxy):**
   - NGINX is installed to handle secure communication and act as a reverse proxy for Loki and Grafana.
   - It terminates SSL/TLS, ensuring secure data transfer between clients and the server.
   - NGINX also plays a crucial role in securing access to Loki by enforcing HTTP Basic Authentication.

#### 7. **Grafana (Visualization Platform):**
   - Grafana is a powerful platform for monitoring and observability.
   - It is configured with Loki as a datasource, allowing users to visualize and explore log data.
   - SSL/TLS is enforced for secure communication with Grafana.

#### 8. **Promtail (Log Shipper):**
   - Promtail is responsible for scraping log files and sending them to Loki.
   - It is configured with targets for system logs, NGINX logs, and Grafana logs.

#### 9. **Self-signed Certificates:**
   - Self-signed TLS certificates are generated for secure communication within the architecture.
   - These certificates are used for NGINX and other components that require secure connections.

### Workflow

1. **EC2 Initialization:**
   - During instance initialization, a user data script is executed to perform setup tasks.
   - Users for Nginx, Promtail, Loki, and Grafana are created, ensuring a secure environment.

2. **Loki Configuration:**
   - Loki is installed and configured with settings for S3 storage, schema, and compaction.
   - Systemd service units are created to manage the Loki service.

3. **NGINX Configuration:**
   - NGINX is installed and configured as a reverse proxy with SSL/TLS termination.
   - Self-signed certificates are generated to secure communication.

4. **Grafana Configuration:**
   - Grafana is installed and configured with Loki as a datasource.
   - Grafana settings, including admin credentials and SSL/TLS configurations, are defined.

5. **Promtail Installation:**
   - Promtail is installed and configured with targets for scraping logs from various sources.
   - A systemd service unit is created to manage the Promtail service.

6. **Outputs:**
   - Outputs from the CloudFormation stack include essential information such as instance details, public IP address, Loki S3 bucket, Loki and Grafana endpoints, and the DNS name of the EC2 instance.
  
# PLNG Stack Deployment on AWS with CloudFormation

## Quick Start

1. **Prerequisites:**
   - [AWS CLI](https://aws.amazon.com/cli/) installed and configured.
   - [Amazon EC2 Key Pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) created.

2. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/plng-stack-aws-cloudformation.git
   cd plng-stack-aws-cloudformation
   ```

3. **Deploy the Stack:**
   ```bash
   aws cloudformation create-stack --stack-name PLNGStack --template-body file://plng-stack-template.yaml \
   --parameters ParameterKey=BucketName,ParameterValue=your-s3-bucket-name ParameterKey=Region,ParameterValue=your-preferred-region \
   --capabilities CAPABILITY_IAM
   ```

4. **Monitor Stack Creation:**
   ```bash
   aws cloudformation wait stack-create-complete --stack-name PLNGStack
   ```

5. **Access Outputs:**
   - Once the stack is created, view the outputs including Instance ID, public IP, Loki S3 storage bucket, Loki and Grafana endpoints, and EC2 instance DNS name.

## Customization

- Modify the `plng-stack-template.yaml` file to adjust parameters such as bucket name, region, usernames, and passwords as needed.

## Architecture

The PLNG Stack architecture involves an EC2 instance hosting Promtail, Nginx, Loki, and Grafana with S3 storage. Security measures include SSL/TLS, HTTP Basic Authentication, and IAM roles.

## Cleanup

To delete the stack when done:

```bash
aws cloudformation delete-stack --stack-name PLNGStack
aws cloudformation wait stack-delete-complete --stack-name PLNGStack
```

## License

This project is licensed under the [MIT License](LICENSE).

Feel free to contribute, open issues, or provide feedback!

---

Adjust the placeholders like `yourusername`, `your-s3-bucket-name`, and others according to your preferences. Additionally, make sure to include any additional instructions or information that might be relevant for users.

### Benefits

- **Scalability:** The architecture is designed to be scalable, allowing organizations to handle varying amounts of log data by adjusting the resources allocated to the EC2 instance and S3 bucket.
- **Security:** Security measures such as SSL/TLS encryption, HTTP Basic Authentication, and the use of IAM roles enhance the overall security posture of the PLNG Stack.
- **Cost-Effectiveness:** By utilizing AWS services like S3 for log storage, the architecture ensures a cost-effective solution for log management.

### Conclusion

The PLNG Stack architecture provides a robust and scalable solution for log management and visualization on AWS. Leveraging services like EC2, S3, and IAM, it offers a secure and cost-effective approach for organizations looking to centralize and analyze their log data.
