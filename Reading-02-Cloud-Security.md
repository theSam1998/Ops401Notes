# <em> Reading 02 </em> - Cloud Security Architectures and Principles
<b> GPT insertions indicated by 🤖
- [AWS Blog (Reading Material)](https://aws.amazon.com/blogs/architecture/compute-abstractions-on-aws-a-visual-story/)
---
## Summary of Key Findings
- AWS provides a broad array of features that enable vast customization to your specific use case. Whether you like to do most of the management yourself, or want most of the maintenance taken care of for you, amazon has an offering or set of offerings that will meet your needs.
- AWS's history seems to have been studded with moments of taking in customer feedback and responding with a product that meets the broadest arrays of needs in every niche, often tailoring specific products to specific demographics if the customer base reacts with a need for a certain gap to be filled.
- Lambda is a powerful tool that can be used to efficiently automate communication between normally disjointed AWS systems. 
  - Personal note: Look into this next time you're having a tough time getting something done due to the nature of the communication taking place.
- AWS systems can be used in tandem across all ends of the spectrum in order to craft a tailored solution, creating an AWS ecosystem rather than an array of separate tools and platforms.
---
## Questions
- Explain the levels of abstraction in AWS to someone without a technical background.

  - VM abstraction: 
    - This abstraction falls directly in the middle between the consumer space and provider space. VM abstraction services include AWS EC2 and Lightsail. At this level, amazon manages the hardware and hypervisor for the instances themselves, While you must manage whatever is done with those instances; For example, you can spin up an ubuntu server in EC2, but it is not going to function as a webpage or host your webserver until you do the work you would need to do with any server, whether physical or virtual, to configure it. 

  - Container abstraction: 
    - This is a higher level of abstraction than the VM level. A VM, as its name implies, is a "virtual machine"; a software emulation of physical hardware on a host machine, designed to run an independent operating system. A container, on the other hand, is focused on providing an isolated environment for your application to run; it packages the application and its necessary dependencies, but it doesn't require a full separate OS like a VM. For instance, referring to the imaginary EC2 webserver scenario, instead of spinning up an entire EC2 instance and setting up the webserver and its files, you could deploy a container. This container encapsulates the necessary components of the OS and other dependencies, along with your webpage data. The result? You're not tying up the resources of an entire VM for just one webpage, optimizing resource use, cost, and deployment speed. Amazon's ECS (Elastic Container Service) was introduced as a managed container orchestration service that natively integrates with Docker. With ECS, Amazon aimed to simplify the orchestration and management of containers for the user, placing more of the responsibility on the service provider, thereby easing the customer's operational load.

      - From the source; "Just like for ECS, the aim for this service is to free AWS customers from having to manage a containers control plane. In the past, AWS customers would spin up EC2 instances and deploy/manage their own Kubernetes masters (masters is the name of the Kubernetes hosts running the control plane) on top of an EC2 abstraction. However, we believe many AWS customers will leave to AWS the burden of managing this layer by either consuming ECS or EKS, depending on their use cases."


  - Function abstraction:
    - Once again a higher level of abstraction than the one before, with AWS shouldering most of the difficult aspects of management, the function abstraction was introduced with AWS Lambda. Lambda is designed to allow you to run your function as a standalone (hence the name "function" abstraction) piece of software, without worrying about any of the OS, dependencies, anything like that; You provide the code, AWS will figure out what is needed to make it run at scale from there. One of the big keys to Lambda's success is the way it is built; it uses what is called an "event-driven model". This architecture allows you to trigger a lambda function when an event occurs in an entirely different area of your aws network (someone places an item in their cart in your online store hosted in an EC2 server, a lambda function is triggered that automatically calculates the updated total price, checks inventory levels for the added item, and sends a personalized notification to the user's registered email or mobile device confirming the item has been added to their cart along with a recommendation for a complementary product based on their browsing history.) In this way Lambda finds its usefulness by serving to efficiently connect systems that would otherwise be difficult to communicate between or entirely disconnected.

  - Bare metal abstraction:
    - A recent development, this abstraction is on the exact opposite end of the spectrum from lambda; most of the work needed to get the system up and running is on you, but you are provded an increased amount of control over the system in its entirety as a result. Introduced at re:Invent 2017, EC2 bare metal instances give the end user direct access to the underlying hardware, bypassing the use of a hypervisor altogether and offering enhanced performance and advanced customization options for programs or systems that may be required to take on a heavier workload. I had a bit of trouble understanding the nature of this abstraction, and the article doesn't clarify very much about it, so I asked ChatGPT for its thoughts on the matter, and I found the observations it provided quite helpful:

    - I first asked for a broad explanation of bare metal ec2 instances.

      - 🤖: Certainly! EC2 Bare Metal instances are a type of Amazon EC2 instance offering that provides direct access to the underlying physical hardware, without any hypervisor overhead. Here's a more detailed breakdown:

        1. **Traditional EC2 Instances**: In most Amazon EC2 instances, there's a layer of virtualization (a hypervisor) that allows multiple instances (or virtual machines) to run on a single physical server. This virtualization layer abstracts and isolates each virtual machine from the others, ensuring they don't interfere with each other.

        2. **Bare Metal Instances**: With EC2 Bare Metal instances, there's no hypervisor layer. Instead, you're getting direct access to the underlying server's hardware. This means your applications can leverage the full power and features of the server's hardware, without any interference or overhead from a hypervisor.

        **Why use Bare Metal Instances?** 

        - There are several reasons why certain workloads might benefit from Bare Metal instances:

          - **Performance**: Without the hypervisor overhead, certain applications can achieve better performance.
  
          - **Specialized Workloads**: Some applications require specific hardware features that aren't always fully exposed or accessible through a hypervisor. With bare metal, you can tap into these features directly.

          - **Licensing**: Some software licenses are tied to physical cores or sockets. Using bare metal can simplify license management for such software.

          - **Virtualization and Nested Virtualization**: If you want to run your own hypervisor or if you need nested virtualization (running a VM inside a VM), then bare metal is the way to go.

        **Types of EC2 Bare Metal Instances**: Amazon offers different types of bare metal instances tailored to different workloads, from memory-intensive applications to storage-optimized tasks.

        - In essence, EC2 Bare Metal instances give you the flexibility of cloud computing but with the capability to run workloads as if they were on a dedicated server, granting direct access to the hardware. It's like having the power of a dedicated server combined with the scalability and features of the AWS cloud.

    - I then asked for further clarification on the distinction between physical and direct acces:

      - 🤖: Haha, no, Amazon doesn't give you a physical pass to their data centers, nor do they mail the hardware to you. When we talk about "direct access to the hardware" in the context of EC2 Bare Metal instances, we're discussing it from a software and computational perspective, not physical access. Here's what "direct access to the hardware" means in this context:

        1. **No Hypervisor Overhead**: Traditional virtualized instances run on top of a hypervisor, which is a layer that manages and mediates access to the underlying physical hardware. With bare metal instances, there's no hypervisor layer. Your software and applications run directly on the physical server, so there's no middleman.

        2. **Full Hardware Features**: Some advanced applications or specialized workloads might require access to specific hardware features, extensions, or instructions that a hypervisor might not expose (or might abstract in a way that's suboptimal). With a bare metal instance, your software can directly interact with and utilize all of these hardware-specific features.

        3. **Dedicated Resources**: Since there's no virtualization overhead, the resources (like CPU, memory, and I/O) are not being shared with other virtual instances. This means that the instance can leverage the full capabilities of the hardware.

        - All of this is managed remotely. You interact with EC2 Bare Metal instances the same way you'd interact with any other EC2 instance: via the AWS Management Console, AWS Command Line Interface (CLI), or SDKs. The difference is in how the instance interacts with the hardware, not how you interact with the instance.
          - 🤖: Example:
          - Imagine you're a gamer. A virtualized instance is like playing a game on a gaming console with certain limitations and abstractions. In contrast, a bare metal instance is akin to playing a game on a custom-built gaming PC, where you can fine-tune hardware components for performance and have direct interaction with them.
          - While you might still play the game using the same controller (similar to using the AWS Console), the experience and performance can differ based on the underlying system.

  - Full container abstraction:
    - A branch off the container abstraction, the full container abstraction seeks to fill the gaps left by its parent. ECS and EKS do not manage the data plane on their own, rather they remove the burden of managing the control plane from the customer. While some users enjoy the ability to manage their own servers within the data plane, others have found this process tedious and wanted a solution that didn't require that enhanced level of attention. The full container abstraction, in the form of AWS Fargate, grants users that option. It allows them to manage solely the containers themselves without worrying about either plane, and further shifts responsibility off the consumer. 

      - 🤖: In simpler terms: Imagine you’re baking a cake (container). Initially, AWS gave you an oven (control plane) but asked you to build your kitchen (data plane) to put the oven in. With Fargate, AWS is now saying, "Don't worry about building the kitchen; just focus on baking your cake. We'll provide the kitchen and the oven."

- What are the control plane and data plane responsible for in container abstraction?
  - The control plane essentially serves as the brain, the decision-making center of the container abstraction. It handles definition, deployment and lifecycling of containers. This means it decides what should be run on which hardware and when it should be run, and so on.
    - 🤖: For a container service like ECS or EKS, the control plane would decide which specific machine (or EC2 instance) a container should run on, when to scale the number of containers, and how to handle if a container crashes.
  -The data plane serves as the body; the cpu, the memory, storage and networking. Your EC2 instance (or network of instances, with all their configuration attached) is the data plane in this scenario, as it is the actual foundation on which the containers in ECS or EKS would be deployed.

- Where does AWS Lambda fall in the layers of abstraction and what makes it so special?
  - Lambda falls to the farthest side of the provider space, with the vast majority of its processes being handled by amazon. Lambda's event-driven architecture, as I mentioned earlier, is a special feature, allowing it to coordinate actions and deploy functions and code across your AWS network, connecting disjointed systems efficiently. Event-driven architecture simply means that when an event that is tied to a trigger occurs, this prompts a responding system (in this case AWS Lambda) to react with a specific action in a specific area depending on what that event was. This type of architecture allows for automated communication on such an instantaneous level that it enables features to be utilized that would otherwise be impossible.
    - 🤖: 

---
## Things I Want to Know More About
- What are some recent and impressive technologies that function by utilizing the entirety (or most of) the AWS ecosystem?
  - 🤖: 
    1. Formula 1's Race Strategy Application: Formula 1 (F1) has developed a race strategy application that uses various AWS services for real-time data analysis. This application relies on Amazon SageMaker for machine learning models, Amazon Kinesis for real-time data streaming, and Amazon S3 for data storage. It processes massive amounts of telemetry data from the cars to provide real-time insights to teams, helping them make strategic decisions during races.
    2. NASA's Mars Rover Missions: NASA's Mars rover missions, such as the Curiosity and Perseverance rovers, utilize a wide range of AWS services. These missions leverage Amazon S3 for data storage, Amazon EC2 for scalable compute power, and Amazon CloudFront for content delivery. AWS's infrastructure supports the processing and analysis of data collected from the surface of Mars, aiding in scientific exploration and discovery.

These examples highlight the diverse ways in which AWS technologies are applied in real-world scenarios, demonstrating the power and flexibility of the AWS ecosystem in various fields, from sports analytics to space exploration.
- When would you want to use a container, and when would this actually make things less efficient?
  - 🤖: When to Use Containers:
    - Microservices Deployment: Containers are well-suited for breaking down monolithic applications into microservices, allowing for independent development, scaling, and deployment of each service.
    - Development and Testing: Containers are highly effective for creating isolated, consistent environments for software development and testing. Developers can package applications and dependencies, ensuring they work the same way across different environments.
  - When Containers Might Be Less Efficient:
    - Small-Scale, Monolithic Applications: For small, single-service, monolithic applications with minimal scaling requirements, containerization might introduce unnecessary complexity. Simpler deployment methods could be more efficient.
    - Legacy Applications: Porting complex legacy applications to containers can be time-consuming and may not offer significant benefits. In such cases, maintaining the existing environments may be more efficient.
- What are the most common use cases for bare-metal instances?
  - 🤖: Common use cases for bare-metal instances, which are physical servers provided by cloud providers, include:
    - High-Performance Workloads: Bare-metal instances are ideal for high-performance, compute-intensive workloads, such as data analytics, scientific simulations, and rendering. They offer the full power of dedicated hardware without the virtualization overhead.
    - Security and Compliance: In industries with strict regulatory requirements, such as finance and healthcare, bare-metal instances are used to ensure data isolation and meet compliance standards. These instances can provide enhanced security and control over the physical infrastructure.
    - Legacy Applications: Some older applications are not designed for virtualized environments and may perform better on dedicated, physical hardware. Bare-metal instances allow organizations to run these legacy applications in the cloud while maintaining their existing architecture.
    - Bare-metal instances are typically chosen when organizations require the highest levels of performance, control, and isolation for their workloads.
