***WEEK 7:LAMBDA***

Step-by-Step Execution (Follow exactly)
🔹 Step 1: Open Lambda
Go to AWS Console
Search: Lambda
Click Lambda service
🔹 Step 2: Create Function
Click Create function
Select: Author from scratch

Fill details:

Function name → basic-python-lambda
Runtime → Python 3.9 (or latest)
Architecture → x86_64

👉 Permissions:

Select Use existing role
Choose Lab Role

Click Create function

🔹 Step 3: Understand Default Code

You’ll see this code:

import json
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
Quick understanding (for viva):
event → input data
context → runtime info
lambda_handler → main function
statusCode → response status
body → output
🔹 Step 4: Replace Code (IMPORTANT)

Delete default code and paste this:

import json

def lambda_handler(event, context):
    a = event.get('num1', 0)
    b = event.get('num2', 0)
    result = a + b

    return {
        'statusCode': 200,
        'body': json.dumps({
            'sum': result
        })
    }

👉 This code:

Takes num1 and num2
Adds them
Returns result
🔹 Step 5: Deploy

Click Deploy
✔️ Wait for "Successfully deployed"

🔹 Step 6: Create Test Event
Click Test
Click Create new event

Fill:

Event name → testInput

Paste this JSON:

{
  "num1": 10,
  "num2": 20
}

Click Test

🔹 Step 7: Output (VERY IMPORTANT)

You should see:

{
  "statusCode": 200,
  "body": "{\"sum\": 30}"
}

✔️ This means SUCCESS

📌 If you see this → experiment done ✔️

🔹 Step 8: Configuration (if asked)

Go to:

General Configuration → Edit

Set:

Memory → 128 MB
Timeout → 3 sec
Architecture → x86_64





***WEEK:8 SNS,SQS***


Steps (Browser only — AWS Console)
1. Create SNS Topic
Go to AWS → Search SNS
Click Topics → Create topic
Type: Standard
Name: MyEmailTopic
Create
2. Create Email Subscription
Open topic → Subscriptions → Create subscription
Protocol → Email
Enter your email
Click Create
3. Confirm Subscription ⚠️

👉 VERY IMPORTANT

Go to email inbox
Click Confirm subscription
4. Publish Message
Open topic → Click Publish message
Subject → Test Mail
Message → Hello this is SNS test
Click Publish

✔️ You’ll receive email

🎯 Viva Points
SNS = Pub/Sub system
Sends same message to multiple subscribers
Protocols: Email, SMS, HTTP








***PART 2: SQS – Queue Messaging***
💡 Concept

Using Amazon Web Services SQS:

Messages stored in queue
Processed later
Helps avoid system overload
🚀 Steps
1. Create Queue
Search SQS
Click Create queue
Type: Standard
Name: MyQueue
Create
2. Send Message
Open queue → Send and receive messages
Message:
Hello this is SQS test message
Click Send
3. Receive Message
Click Poll for messages
✔️ You’ll see message
🎯 Viva Points
SQS = Message queue
Used for decoupling systems
Stores messages reliably



***🔶 PART 3: FULL ARCHITECTURE (MOST IMPORTANT 🔥)***
🧠 Concept Flow
S3 (Upload file)
   ↓
SNS (Send notification)
   ↓
SQS (Store message)
   ↓
Lambda (Process message)

👉 This is called event-driven serverless architecture

🚀 Steps (Simplified for lab)
✅ Step 1: Create S3 Bucket
Go to S3
Create bucket:
Name: mys3eventbucket123 (unique)
Region: SAME as SNS ⚠️
Create
✅ Step 2: Create SNS Topic
Name: MyS3SNSTopic
✅ Step 3: Create SQS Queue
Name: MyS3Queue
✅ Step 4: Connect SNS → SQS
Open SNS Topic
Create subscription:
Protocol → SQS
Select your queue
✅ Step 5: Configure S3 → SNS
Open S3 bucket → Properties
Event notifications → Create
Event: All object create
Destination: SNS topic
✅ Step 6: Test (IMPORTANT)
Upload file to S3

✔️ What happens:

S3 sends event
SNS sends message
SQS stores message
✅ Step 7: Check SQS
Open queue
Poll messages
✔️ You’ll see event data
✅ Step 8: Add Lambda (Final step)
Create Lambda:
Name: SQSConsumerFunction
Runtime: Python
Add trigger:
Select SQS
Choose your queue
Replace code:
def lambda_handler(event, context):
    for record in event['Records']:
        print("Message received from SQS:")
        print(record['body'])

    return {
        'statusCode': 200,
        'body': 'Message processed successfully'
    }
Click Deploy
🎯 FINAL RESULT (Say in viva)

👉 When file is uploaded:

S3 triggers event
SNS sends notification
SQS stores message
Lambda processes it




***WEEK:9 LOAD BALANCER***

🧩 PART 1: Create EC2 Servers
💡 Concept

Using Amazon Web Services EC2:

These are your web servers
🚀 Steps
1. Launch Instance 1
Go to EC2 → Launch Instance
Name → webserver-1
AMI → Amazon Linux 2
Instance → t2.micro
Security Group:
SSH (22) → My IP
HTTP (80) → Anywhere

Launch → Download key → Launch

Connect & Run Commands (IMPORTANT ⚠️)

👉 Use EC2 browser terminal OR Git Bash (not Kali)

yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "This is Server 1" > /var/www/html/index.html


2. Launch Instance 2

Repeat same steps
Change last line:

echo "This is Server 2" > /var/www/html/index.html
✅ Test
Copy public IP → open browser
✔️ You should see Server 1 / Server 2 page
🧩 PART 2: Create Load Balancer (ALB)
💡 Concept

👉 Elastic Load Balancing distributes traffic

🚀 Steps
1. Create Security Group
Name → lb-sg
Allow HTTP (80)
2. Create Load Balancer
EC2 → Load Balancers → Create
Type → Application Load Balancer

Fill:

Name → my-alb
Scheme → Internet-facing
Listener → HTTP (80)
3. Select VPC + Subnets
Choose default VPC
Select 2 subnets
4. Attach Security Group
Select lb-sg
5. Create Target Group
Name → web-servers-tg
Type → Instance
Protocol → HTTP

👉 Register:

webserver-1
webserver-2
6. Create Load Balancer
✅ Test (VERY IMPORTANT)
Copy DNS name of ALB
Paste in browser

👉 Refresh multiple times:

You’ll see:
Server 1
Server 2

✔️ That means Load Balancer works

🧩 PART 3: Auto Scaling (MOST IMPORTANT 🔥)
💡 Concept

👉 Auto Scaling:

Automatically adds/removes servers based on load
🚀 Steps
✅ Step 1: Create AMI
EC2 → Instances
Select instance
Actions → Create Image

Name → my-webserver-AMI

✅ Step 2: Create Launch Template
EC2 → Launch Templates
Name → my-launch-template
Select AMI
Instance → t2.micro
Security Group → HTTP allowed
✅ Step 3: Create Auto Scaling Group
Name → webserver-asg
Select launch template
Network:
Choose VPC
Select subnets
Attach Load Balancer:

👉 VERY IMPORTANT

Select existing Load Balancer
Capacity:
Desired → 2
Min → 1
Max → 4

Create ASG

✅ Test Auto Scaling
Test 1: Normal
Go to ALB → DNS
Refresh → multiple servers
Test 2: Auto Recovery 🔥
Terminate one EC2 instance

👉 After few seconds:
✔️ New instance will be created automatically

🎯 FINAL FLOW (Say in viva confidently)
User → Load Balancer → EC2 Instances
                        ↑
                  Auto Scaling adds/removes servers





***WEEK:10 ELASTIC BEAN STACK***


STEP-BY-STEP EXECUTION
🔹 Step 1: Open Beanstalk
Go to AWS Console
Search: Elastic Beanstalk
Click Create Application
🔹 Step 2: Create Application
Name → MyApp
Click Create
🔹 Step 3: Create Environment
Click Create Environment
Choose:
Web Server Environment
🔹 Step 4: Configure
Basic:
Environment name → my-env
Platform:

Choose based on your app:

Python / Node.js / Tomcat / PHP

👉 If you don’t know:
✔️ Select Python (safe option)

Upload Code:
Select Upload your code
Upload .zip file

👉 If no code:

Use Sample application (quick hack for lab)
🔹 Step 5: Service Access
Role → LabRole
Instance profile → LabInstanceProfile
🔹 Step 6: Network
VPC → default
Subnets → default
Enable public IP
🔹 Step 7: Scaling
Instance → t2.micro
Min → 1
Max → 2
Load Balancer → enabled
🔹 Step 8: Create
Click Review → Create

⏳ Wait 2–5 minutes

✅ OUTPUT (VERY IMPORTANT)

After deployment:

You’ll get a URL

👉 Open it → Your app runs 🎉

✔️ That means SUCCESS





***WEEk 11:AMAZON LEX***

STEP-BY-STEP EXECUTION
🔹 Step 1: Open Lex
Go to AWS
Search Amazon Lex
Click Create bot
🔹 Step 2: Create Bot
Select → Create blank bot
Name → HotelBookingBot
IAM role → Create new
Language → English
Click Done
🔹 Step 3: Create Intent

👉 Intent = user goal

Go to Intents
Create → BookHotel
🔹 Step 4: Add Utterances

👉 These are user sentences

Add:

I want to book a hotel
Book a room
Reserve hotel
I need a room
🔹 Step 5: Add Slots (VERY IMPORTANT)

👉 Slots = user inputs

Slot 1: Age
Name → age
Type → AMAZON.Number
Prompt → “What is your age?”
Required → Yes
🔥 Add Condition (Important for viva)

If age < 18:
👉 “You are not eligible”

Slot 2: Location
Type → AMAZON.City
Prompt → “Which city?”
Slot 3: Check-in
Type → AMAZON.Date
Prompt → “Check-in date?”
Slot 4: Nights
Type → AMAZON.Number
Prompt → “How many nights?”
🔹 Step 6: Custom Slot (Room Type)

👉 Create Slot Type:

Name → RoomType
Values:
Single
Double
Suite

Then add slot:

Slot name → roomtype
Slot type → RoomType
🔹 Step 7: Add Buttons (Optional but good)

👉 Response cards:

Single
Double
Suite
🔹 Step 8: Responses

Initial:
👉 “Welcome to Hotel Booking!”

Confirmation:
👉 “Confirm booking in {location} for {nights} nights?”

🔹 Step 9: Build & Test 🔥
Click Build
Open test panel

Try:

Book a hotel
✅ EXPECTED OUTPUT

Conversation like:

Bot: What is your age?
User: 25
Bot: Which city?
User: Hyderabad
Bot: Check-in date?
User: Tomorrow
Bot: Nights?
User: 2
Bot: Room type?
User: Double
Bot: Confirm booking?

✔️ That means SUCCESS







***WEEK 12: IAM USER(GUI AND CLI)***


PART A: GUI ACCESS (Web Login)
💡 Concept

👉 Create user → Give limited permission → Test access

🚀 Steps
🔹 Step 1: Login as Root
Open AWS Console
Login using root account
🔹 Step 2: Open IAM
Search IAM
Go to Users → Create user
🔹 Step 3: Create User
Name → S3_Specialist
Enable:
✔️ Management Console access
Set password
🔹 Step 4: Set Permissions
Select:
👉 Attach policies directly
Choose:
👉 AmazonS3FullAccess
🔹 Step 5: Create & Download CSV ⚠️
Click Create
Download .csv (VERY IMPORTANT)

👉 Contains:

Username
Password
Login URL
🔹 Step 6: Logout Root
🔹 Step 7: Login as IAM User
Use Sign-in URL
Enter:
Account ID
Username
Password
✅ TEST (VERY IMPORTANT)
❌ Test 1 (EC2)
Open EC2
👉 You’ll see Access Denied
✅ Test 2 (S3)
Open S3
Create bucket

✔️ Works

🎯 What this proves

👉 User has:

S3 access ✅
EC2 access ❌
🔥 PART B: CLI ACCESS
💡 Concept

👉 Access AWS using terminal commands

🚀 Steps
🔹 Step 1: Generate Keys
IAM → Users → Select user
Security credentials
Create access key
Select CLI

👉 Download CSV

🔹 Step 2: Install CLI

👉 Install AWS CLI from Google

🔹 Step 3: Configure CLI

Open CMD / Terminal:

aws configure

Enter:

Access Key
Secret Key
Region → ap-south-1
Output → json
🔹 Step 4: Test Commands
✅ Command 1
aws s3 ls

✔️ Shows buckets

✅ Command 2
aws s3 mb s3://your-unique-name

✔️ Creates bucket

❌ Command 3
aws iam list-users

👉 ❌ Access Denied











***WEEK 13: IAM ROLES***
STEP-BY-STEP EXECUTION
🔹 Step 1: Create IAM Role
Go to IAM → Roles → Create role

Select:

Trusted entity → AWS Service
Use case → EC2
🔹 Step 2: Add Permissions
Search → AmazonS3FullAccess
Select it
🔹 Step 3: Name Role
Name → EC2-S3-Role
Create role
🔹 Step 4: Attach Role to EC2
Go to EC2
Select running instance

👉 Actions → Security → Modify IAM role

Select → EC2-S3-Role
Click Update
🔹 Step 5: Test in EC2 (IMPORTANT)

👉 Connect to EC2 (browser terminal or SSH)

✅ Test 1 (S3 works)
aws s3 ls

✔️ Shows buckets

❌ Test 2 (EC2 fails)
aws ec2 describe-instances

👉 ❌ Access Denied
