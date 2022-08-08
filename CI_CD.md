# Continuous Integration/ Delivery/ Deployment

### Most of the CI/CD pipelines will have the following steps. Below steps gives an idea of the building blocks of a typical CICD pipeline.
- We'll trigger the pipeline by pushing an update to the Git repository where the application code is stored. 
- Or, in some cases, we may trigger the pipeline manually if needed. 
- In the first two stages, we'll put any required libraries in place and do a sanity check. 
- We'll scan our code against linters, and run some unit tests to make sure the specification has been followed correctly.
- It makes sense to do these checks early in the pipeline to keep from building and deploying bad code.
- This is the integration part of our CI/CD pipeline. At this stage, we can also include additional tests that improve the quality of our application. 
- We could run security tests by a scan for vulnerabilities or checking to make sure secrets like passwords or API keys have not been committed to the repository.
- In the third stage, we'll build the application.
- We'll bundle the contents of the working directory into a ZIP file and upload the ZIP to an S3 bucket.
- Then we'll trigger Elastic Beanstalk to store the bundle as a new version of our app.
- This step is the delivery portion of our CICD pipeline since we're making a new version available to deployed.
- We'll use stages four and five to deploy our application to a staging environment.
- In this environment, we'll be able to run tests on the application before it goes live in production.
- If those tests pass, then our application will be ready for the big time.
- In the last stages of the pipeline, we can confidently deploy our application to a production environment.
- By this time, the code has been integrated, linted, tested at the unit level, built into a deliverable artifact, and deployed and tested in a staging environment. 
- But just to make sure nothing went wrong, we'll run one more test on the live site. 

### CI/CD tool categories
#### Self-hosted
Self-hosted tools run on your hardware. This could mean the tool runs on a server in your company's data center, a VM in the cloud, or it could be on your local workstation. Whatever the platform you are responsible for installing the tool and maintaining it.
  - Jenkins
  - Bamboo
  - TeamCity
  - GoCD
#### Software as a service (SaaS)
Tools that fit into the software as a service or SaaS category, offer an alternative to self-hosting. In this case, a vendor provides and maintains the tool and allows you to access it.
  - Travis CI
  - Codeship
  - Circle CI
#### Cloud Service Providers
The next category is an extension of the software as a service category, cloud service providers offer SaaS-based CI CD tools but they also offer other cloud-based services like virtual machines, container registries, and cloud storage.
  - Amazon Web services (AWS) CodePipeline and CodeBuild
  - Azure Pipelines
  - Google Cloud Platform (GCP) Build
#### Code repositories
If you think of a code repository, your first thought is probably of a place where you can store your code. But along with giving you a place to track and manage your code, many modern repositories also provide CI/CD tools for turning that code into software.
  - GitHub Actions
  - GitLab CI
  - Bitbucket Pipelines

