Quick Start
Prerequisites:

AWS CLI installed and configured.
Amazon EC2 Key Pair created.
Clone the Repository:

bash
Copy code
git clone https://github.com/yourusername/plng-stack-aws-cloudformation.git
cd plng-stack-aws-cloudformation
Deploy the Stack:

bash
Copy code
aws cloudformation create-stack --stack-name PLNGStack --template-body file://plng-stack-template.yaml \
--parameters ParameterKey=BucketName,ParameterValue=your-s3-bucket-name ParameterKey=Region,ParameterValue=your-preferred-region \
--capabilities CAPABILITY_IAM
Monitor Stack Creation:

bash
Copy code
aws cloudformation wait stack-create-complete --stack-name PLNGStack
Access Outputs:

Once the stack is created, view the outputs including Instance ID, public IP, Loki S3 storage bucket, Loki and Grafana endpoints, and EC2 instance DNS name.
Customization
Modify the plng-stack-template.yaml file to adjust parameters such as bucket name, region, usernames, and passwords as needed.
Architecture
The PLNG Stack architecture involves an EC2 instance hosting Promtail, Nginx, Loki, and Grafana with S3 storage. Security measures include SSL/TLS, HTTP Basic Authentication, and IAM roles.

Cleanup
To delete the stack when done:

bash
Copy code
aws cloudformation delete-stack --stack-name PLNGStack
aws cloudformation wait stack-delete-complete --stack-name PLNGStack
License
This project is licensed under the MIT License.

Feel free to contribute, open issues, or provide feedback!