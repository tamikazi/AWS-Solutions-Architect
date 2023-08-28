# Machine Learning
## Rekognition
- Find objects, people, text, scenes in images and videos using ML
- Facial analysis and facial search to do user verification, people counting
- Create a database of “familiar faces” or compare against celebrities
- Use cases:
	- Labeling
	- Content Moderation
	- Text Detection
	- Face Detection and Analysis (gender, age range, emotions…)
	- Face Search and Verification
	- Celebrity Recognition
	- Pathing (ex: for sports game analysis)
## Transcribe
- speech to text
- can remove personally identifiable information
## Polly
- text to speech
- create apps that talk
- can use Lexicons and SSML for pronunciation and additional customization
## Translate
- natural and accurate language translation
## Lex & Connect
- Lex: same as Alexa, speech recognition
	- chatbots and call centers
- Connect: 
	- Receive calls, create contact flows, cloud-based virtual contact center
	- Can integrate with other CRM systems or AWS
	- No upfront payments, 80% cheaper than traditional contact center solutions
## Comprehend
- Natural Language Processing
- find insights and relationships in text
- analyze customer emails
### Medical
- detects and returns useful info in unstructured clinical text
## SageMaker
- build ML models\
## Forecast
- ML to deliver accurate forecasts
- predict future sales
## Kendra
- document search service using ML
- extract answers from within a document
## Personalize
- build apps with real time personalized recommendations
- for personalized product recommendations, customized marketing
## Textract
- extracts text, handwriting and data from scanned documents

# CloudFormation
- declarative way of outlining AWS infrastructure
- creates everything in the right order
- infrastructure as code
- Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
- Productivity
	- Ability to destroy and re-create an infrastructure on the cloud on the fly
	- Automated generation of Diagram for your templates!
	- Declarative programming (no need to figure out ordering and orchestration)
# Simple Email Service (SES)
- fully managed service to send emails securely and globally at scale
# Pinpoint
- Scalable 2-way (outbound/inbound) marketing communications service
- Supports email, SMS, push, voice, and in-app messaging
- Ability to segment and personalize messages with the right content to customers
- Possibility to receive replies
## Versus Amazon SNS or Amazon SES
- In [[Scalable and loosely coupled architectures|SNS]] & SES you managed each message's audience, content, and delivery schedule
- In Amazon Pinpoint, you create message templates, delivery schedules,
# Systems Manager
## Session Manager
- allows you to start a secure shell on instances and on premise servers
## Run Command
- execute document or just run a command
- No need for SSH
- Command Output can be shown in the AWS Console, sent to S3 bucket or CloudWatch Logs
- Send notifications to [[Scalable and loosely coupled architectures|SNS]] about command status (In progress, Success, Failed, …)
- Integrated with [[Secure access to AWS resources|IAM]] and [[Scalable and loosely coupled architectures|CloudTrail]]
- Can be invoked using [[Scalable and loosely coupled architectures|EventBridge]]
## Patch Manager
- automates patching
## Maintenance Windows
- defines schedule for when to perform actions on your instances
## Automation
- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- Examples: restart
# Cost Explorer
- visualize costs
- savings plan
# Elastic transcoder
- convert media file formats in [[Scalable storage solutions|S3]]
# Batch
- fully managed batch processing at any scale
- dynamically launch instances or Spot instances
- defined as Docker images
- helpful for cost optimizations
# AppFlow
- securely transfer data between SaaS apps and AWS like **Salesforce**, SAP, Slack, etc
- destinations: [[Scalable storage solutions|S3]], [[Database solutions|Redshift]]
# Amplify
- A set of tools and services that helps you develop and deploy scalable full stack web and mobile applications
# Well Architected Framework General Guiding Practices
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
- Design based on changing requirements
- Drive architectures using data
- Improve through game days
- Simulate applications for flash sale days
## 6 Pillars
1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability
## Tool
- review architectures against the 6 pillars and develop best practices
## Trusted Advisor
- high level account assessment
### Categories
- Cost Optimization
- Performance
- Security
- Fault Tolerance
- Service Limits

