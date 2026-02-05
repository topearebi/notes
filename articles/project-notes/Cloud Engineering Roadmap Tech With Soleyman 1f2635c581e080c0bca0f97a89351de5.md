# Cloud Engineering Roadmap| Tech With Soleyman

## Summary

### I. Master IT Fundamentals

This is the bedrock upon which your cloud career will be built.  A strong grasp of these fundamentals is crucial.

- **Operating Systems:**
    - **Linux:** Become proficient in Linux. It's the dominant OS in cloud environments. Focus on command-line navigation, file system management, and user/permission management. Distributions like Ubuntu or CentOS are good starting points. [30 Days of Linux : A Complete Guide For Beginners](https://www.notion.so/30-Days-of-Linux-A-Complete-Guide-For-Beginners-1ec635c581e0806c8d55f5e80060edf8?pvs=21)
    - **Windows:** While less prevalent in the cloud, Windows still holds a significant presence. Understand its core functionalities and how it differs from Linux.
    - **Pitfalls:** Ignoring the command line, neglecting user and permission management.
    - **Tips:** Set up a virtual machine (using VirtualBox or VMware) to practice Linux commands and explore different distributions.
- **Scripting:**
    - Learn a scripting language like Bash (for Linux) or PowerShell (for Windows). Automation is key in the cloud.
    - **Pitfalls:** Not practicing scripting enough to become comfortable with it.
    - **Tips:** Automate everyday tasks on your computer using scripts to gain practical experience.
- **Networking:**
    - Understand the fundamentals of how computers communicate, including the OSI model, TCP/IP, and common protocols (HTTP, DNS).
    - **IP Addressing:** Master the concepts of IPv4 and IPv6 addressing, subnetting, and routing.
    - **Pitfalls:** Getting bogged down in complex networking theory without practical application.
    - **Tips:** Use online tools and visualisations to understand network concepts. Set up a simple home network to experiment with IP addressing and routing.
- **Security:**
    - Learn about common security threats and vulnerabilities in IT systems.
    - **Firewalls:** Understand how firewalls work and how to configure them.
    - **VPNs:** Learn about VPNs and their role in secure communication.
    - **Pitfalls:** Underestimating the importance of security in the cloud.
    - **Tips:** Explore online resources and courses dedicated to cybersecurity fundamentals.
- **Programming:**
    - **Python:** Python is widely used in cloud automation and scripting. Learn the basics of Python syntax, data structures, and working with libraries.
    - **Pitfalls:** Trying to become a Python expert before moving on to cloud-specific learning.
    - **Tips:** Focus on practical Python skills relevant to cloud tasks, such as working with APIs and automating infrastructure.
- **Databases:**
    - **SQL:** Learn the basics of relational databases and SQL for querying and managing structured data.
    - **NoSQL:** Understand the concepts of NoSQL databases and their use cases for unstructured and semi-structured data.
    - **Pitfalls:** Trying to master advanced database administration before understanding basic cloud concepts.
    - **Tips:** Use online platforms like Mode Analytics or free database instances to practice SQL queries. Explore popular NoSQL databases like MongoDB.
- **Virtualisation:**
    - Understand the concept of virtual machines (VMs) and how they allow multiple operating systems to run on a single physical machine.
    - **Pitfalls:** Confusing virtualisation with containerisation.
    - **Tips:** Set up virtual machines using VirtualBox or VMware to get hands-on experience.
- **Containerisation:**
    - Learn about containers (e.g., Docker) and how they package applications and their dependencies into isolated units.
    - **Pitfalls:** Not understanding the benefits and limitations of containers compared to VMs.
    - **Tips:** Experiment with Docker by containerising a simple application and deploying it locally.

### II. Pick a Cloud Platform

While the underlying concepts are similar, each cloud provider has its own set of services and tools. It's best to focus on one platform initially.

- **AWS (Amazon Web Services):**
    - **Pros:** Largest market share, extensive free tier for experimentation, skills highly transferable.
    - **Cons:** Can be overwhelming due to the sheer number of services, pricing can become complex.
- **Azure (Microsoft Azure):**
    - **Pros:** Strong integration with Microsoft products and services, good for organisations already using Microsoft technologies.
    - **Cons:** Less mature than AWS in some areas, can be more expensive for certain services.
- **Google Cloud Platform (GCP):**
    - **Pros:** Known for innovation in areas like AI/ML and big data, competitive pricing.
    - **Cons:** Smaller market share compared to AWS and Azure, some services less mature.
- **Pitfalls:** Spreading yourself too thin by trying to learn all platforms at once.
- **Tips:** Choose the platform that aligns with your career goals and interests. Leverage the free tier to explore and experiment.

### III. Master Cloud Concepts (Focusing on AWS as an Example)

- **Understand the "why" before the "how"**: Don't just learn how to use a service; understand the underlying problem it solves and its place within the cloud ecosystem.
- **Core Services:**
    - **EC2 (Elastic Compute Cloud):** The foundation for running virtual servers in the cloud. Learn about instance types, operating systems, storage options, and security.
    - **S3 (Simple Storage Service):** Object storage for various data types (images, videos, documents, etc.). Understand storage classes, access management, and data lifecycle policies.
    - **IAM (Identity and Access Management):** Crucial for controlling access to your cloud resources. Learn about users, groups, roles, and policies.
    - **VPC (Virtual Private Cloud):** Allows you to create isolated networks within AWS. Understand subnets, route tables, and security groups.
    - **Bedrock (Generative AI service):** Explore the possibilities of AI in the cloud with pre-built foundation models.
    - **Pitfalls:** Focusing only on the "how-to" without grasping the underlying concepts.
    - **Tips:** Use AWS's extensive documentation and online tutorials. Start with simple use cases and gradually increase complexity.

### IV. Projects

Hands-on experience is essential. Build projects to solidify your understanding and showcase your skills.

- **Portfolio Website:**
    - Host your website files on S3, configure a custom domain, and implement versioning.
    - **Tips:** Use this project to learn about S3 security features, static website hosting, and DNS management.
- **Task Tracking System:**
    - Use an EC2 instance to host a web application (built with Python, for example) that connects to a database (using RDS for a relational database).
    - **Tips:** This project will help you understand application deployment, database integration, and basic web development.
- **Image Sharing App:**
    - Store images in S3. Implement user authentication and authorisation using IAM.
    - **Tips:** Focus on security best practices for handling user data and access control.

### V. Certifications

Industry certifications validate your skills and enhance your credibility.

- **Cloud Practitioner:** A foundational certification that demonstrates basic understanding of cloud concepts.
- **Solutions Architect (Associate/Professional):** Focuses on designing and deploying scalable and reliable systems on AWS.
- **Pitfalls:** Relying solely on certifications without practical experience.
- **Tips:** Use certifications to guide your learning and demonstrate your commitment to the field.

### VI. Essential Cloud Tools

- **Infrastructure as Code (IaC):**
    - **Terraform:** Learn to define and manage your infrastructure using code. This enables automation, reproducibility, and version control.
    - **Pitfalls:** Manually configuring resources through the AWS console, which is not scalable or maintainable.
    - **Tips:** Embrace IaC from the beginning. Practice writing Terraform configurations for your projects.
- **CI/CD Pipelines:**
    - **GitHub and GitHub Actions:** Implement continuous integration and continuous delivery (CI/CD) to automate code deployments and testing.
    - **Pitfalls:** Deploying code manually, which is prone to errors and inconsistencies.
    - **Tips:** Set up automated build and test pipelines for your projects using GitHub Actions.

### VII. Advanced AWS Services

- **AWS Lambda:** Serverless compute service for running code without managing servers.
- **API Gateway:** Create and manage APIs for your applications.
- **DynamoDB:** NoSQL database service for high-scale applications.
- **SageMaker:** Platform for building, training, and deploying machine learning models.
- **Pitfalls:** Jumping into advanced services without a solid foundation in core services.
- **Tips:** Start with simple Lambda functions and gradually explore more complex use cases.

### VIII. Complex Project Building

- **Event-driven Notification System:** Build a system that uses Lambda, DynamoDB, and SNS (Simple Notification Service) to trigger notifications based on events.
- **Book Recommendation Engine:** Develop an AI-powered system that recommends books to users based on their preferences and historical data.
- **Pitfalls:** Undertaking overly ambitious projects too early in your learning journey.
- **Tips:** Break down complex projects into smaller, manageable tasks. Focus on building a functional system first, then iteratively add features.

## Build Online Profile

## Key concepts

1. theme 1
2. theme 2

## Critical Analysis

### Strengths

### Weaknesses

### Disagreements

### Agreements

### Real-world applications

## Connections

### Other literature

### Fleeting notes

### Permanent notes

### Structure notes