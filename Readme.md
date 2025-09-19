#### Project: EBS-Volume-Type-Auto-Conversion-using-AWS-Lambda-and-EventBridge:
#### 🎯 Why I Chose This Project
Every company must follow compliance policies to manage costs and security across different environments. This project demonstrates how AWS Lambda can be used to enforce such organizational compliance efficiently. It reflects a real-world scenario, helping me understand serverless architecture while addressing practical challenges.

#### 📘 Project Description
This project enforces an organizational policy ensuring that every newly created EBS volume uses the gp3 volume type instead of gp2, providing better performance and cost-efficiency. Built with a serverless, event-driven architecture leveraging AWS Lambda, CloudWatch, and boto3.
- AWS Lambda — automatically triggered code that runs without managing any infrastructure
- Amazon EventBridge (CloudWatch Events) — detects the EBS volume creation event and triggers Lambda
- IAM Roles and Policies — to securely allow Lambda to access EC2
- Boto3 (AWS SDK for Python) — to interact with EC2 APIs

#### 🧱 Architecture:
<img width="521" height="367" alt="image" src="https://github.com/user-attachments/assets/31adb70e-164c-463f-b337-2ae3881485c4" />

#### ✅ How It Works:
- A new EBS volume is created (e.g., by EC2 launch or manually).
- EventBridge rule listens for createVolume events from the EC2 service.
- The rule triggers a Lambda function, passing the event as input.
- Lambda function extracts the volume ARN from the event (resources key),Parses the Volume ID from the ARN, Calls ec2.modify_volume() via Boto3 to change the type to gp3.

#### ❓ Design Features of why did I chose AWS Lambda/Cloudwatch Eventbridge/Cloudwatch logs:
- Used Lambda, as there's no infrastructure to manage. AWS handles scaling automatically, so even if multiple volumes are created at the same time, the Lambda will
  handle  them in parallel without any manual intervention and I will pay only for the time executed and only for the number of times it triggered.
- CloudWatch Logs are used to monitor Lambda execution else I don't know the status of the function. This is essential for debugging and verifying whether the function
  ran   successfully, especially since Lambda runs in the background.
- EventBridge (formerly called CloudWatch Events) listens for events like createVolume and routes them to Lambda — since EBS doesn't have built-in trigger
  support,EventBridge acts as the event listener and a middleman between EBS and Lambda.

#### ✅ Prerequisites:
- Set up an AWS Lambda function that gets triggered by an event.
- The event sends data (usually in JSON format) to the Lambda.
- Used Python and the boto3 SDK to interact with AWS services.
- Worked with multiple AWS services, including EC2 – mainly to interact with EBS volumes, Lambda – where the main logic (handler) runs, CloudWatch – to trigger Lambda
  via scheduled events or logs.
- Created IAM roles and permissions, Defined policies with required access and attached policies to roles.

#### 🧠 Lessons Learned:
- Wrote a Lambda handler function that processes event data.
- Understood how event data is structured and passed into Lambda.
- Used boto3 clients to communicate with AWS services (like ec2 for EBS) in python
- Learned how to set up proper IAM roles and policies for secure access.
- Built a serverless workflow that responds to AWS events using Python.

#### 🔒 Best Practices & Challenges
- Used least privilege when assigning IAM roles and permissions.
- Common challenges faced includes short parsing errors in JSON caused by mistakes like not using the correct key names.
- Always check and print the event data in the Lambda handler since you can’t see it directly. Printing it helps you view the data in CloudWatch logs.
- After printing, safely parse the JSON using the correct key names.
- Note that Lambda logs are written to CloudWatch by default, which helps with debugging.

#### 💡 Key Benefits:
 | Feature                     | Description                                                                            |
 | --------------------------- | -------------------------------------------------------------------------------------- |
 | 🖥️ **Serverless (Lambda)**  | No infrastructure to manage. Scales automatically with number of events.               |
 | ⚙️ **Event-Driven**         | Automatically reacts to EBS volume creation in real time.                              |
 | 📊 **CloudWatch Logs**      | Logs are stored in CloudWatch, helping you verify execution and debug issues.          |
 | 🔁 **Auto Scaling**         | Lambda functions run concurrently for multiple EBS events, without any manual scaling. |
 | 🔒 **Secure by Design**     | IAM roles restrict access to only required AWS services.                               |
 | 📉 **Cost Optimization**    | Enforces `gp3` usage to improve performance over `gp2`.                                |

#### Upgrading EBS Volumes to gp3: How It Works on AWS and Why It Matters:
⚙️ How AWS Changes Volume Performance with gp3
- Switching an EBS volume from gp2 to gp3 updates its performance settings without replacing the disk. AWS adjusts IOPS and throughput via software, delivering faster,
  more  consistent I/O while the volume stays attached and online.
- Think of it like increasing speed limits on a highway without rebuilding the road.

💾 What Is EBS Storage?
- EBS volumes look like physical disks to your EC2 instances but are actually virtualized storage managed across multiple physical devices by AWS for
  durability and scalability.

🚀 Why Choose gp3?
- Faster and more consistent IOPS and throughput
- Independent provisioning of performance
- Better cost efficiency than gp2
- In short, gp3 offers better, flexible performance at lower cost.
