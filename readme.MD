# How do you configure DevOps to a new project

As a DevOps engineer our clients and customers are developers. We let developers only to code, remaining responsibilities we will take care, so that they only focus on coding.

As a DevOps engineer our goals are
1. Faster releases
2. Dev to PROD automated deployments
3. Less defects

As a DevOps team we always look for best practices and continuously evolve our process. We are following
* DevSecOps
    We implemented scanning at multiple levels.
    * Static Source code analysis
    * Static application security testing
    * Dynamic application security testing
    * Open source scanning
    * Image and containers scanning
* Shift left <br/>
    We shifted testing and scanning to the DEV stage, so that we can find build related, security related, functionality related errors as early as possible
* Build once and run anywhere <br/>
    We only build once in DEV environment. We promote the same build/image to higher environments including prod but with different configurations.

If any new project is started, we should follow a process.
* Have a meeting with DEV team
* Know their programming language, deployment platform
* Branching strategy, preferred is feature branching if they are into Agile.

Once we discuss our approach and strategy with them, We will onboard them into all the platforms through SRE team.
* Git
    Creating repos, settings(branching strategy, PR process), etc.
* Jenkins
    Creating a folder for their project and RBAC.
* SonarQube
    Creating a project, setting up quality gates, profiles, etc.
* Nexus
    Creating a repo to store their artifacts.
* AWS
    Creating the infra like Route53, New or existing VPC based on their requirement, VM if required, IAM roles, policies, ECR repos and scanning, etc.
* EKS
    Creating the namespace in all environments like DEV, QA, PROD, etc.
* Terraform
    Creating their infra as a code.

Once application presence is created in all the tools and platforms. We onboard them to our pipelines powered by Jenkins Shared Library. These are centralized pipelines created by us for end to end release automation from DEV to PROD. We have different combination of pipelines based on their programming language, deployment platform. We have

* NodeJS/ReactJS into VM and EKS.
* Java into VM and EKS.
* Python into VM and EKS.

We have few pipelines that can support legacy applications too. We ask application team to keep a simple 10lines Jenkinsfile to put in their repo. this file can call our libraries and pass few parameters like project code, component name, etc.

We have 3 kind of pipelines.
* Development pipeline. Stages are
    * Clone
    * Build
    * Unit testing, Sonar quality gate must pass 80% coverage.
    * All types of scans
    * Artifact upload
    * Image upload
    * Deployment into DEV
    * Functional testing

Once DEV pipeline is success, application team can go ahead and raise PR. Once this is approved, they can trigger.

* NON-PROD Pipleine.
    We will pull the image/artifact into NON-PROD environment like QA, UAT, etc. We deploy the application and run the integration tests with the help of QA team.

Once NON-PROD pipeline is success, application team have a process of CR(Change request). They create a CR and get the approvals.

* PROD pipeline
    When our pipeline is triggered, we get the CR number in input we check once the trigger time is in between deployment window and the approval. If everything fine we deploy the application into PROD environment. We follow zero downtime roll out strategy.

Later application team will have sanity testing.

Apart from these we perform EKS upgrades through blue/green approach.