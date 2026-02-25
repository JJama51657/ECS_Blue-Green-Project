<h1>ECS Blue/Green Deployment with Automated CI/CD</h1>

<h2>Project Overview</h2>
<p>This project demonstrates a <strong>production-style blue/green deployment pipeline</strong> for a containerized Java application using <strong>AWS ECS Fargate</strong>, <strong>CodeDeploy</strong>, and <strong>GitHub Actions</strong>. It focuses on <strong>zero-downtime deployments</strong>, <strong>automated rollback</strong>, and <strong>deployment safety</strong>.</p>

<h2>Features</h2>
<ul>
  <li><strong>Blue/Green Deployment:</strong> ECS + CodeDeploy traffic shifting with ALB health checks.</li>
  <li><strong>Continuous Integration (CI):</strong> GitHub Actions builds Docker images, runs Maven tests, performs Trivy security scans, and executes SonarQube static code analysis with Quality Gate enforcement.</li>
  <li><strong>Continuous Deployment (CD):</strong> GitHub Actions pushes Docker images to ECR, updates ECS task definitions, and triggers CodeDeploy Blue/Green deployments.</li>
  <li><strong>Automated Rollback:</strong> CloudWatch alarms trigger rollback on failures.</li>
  <li><strong>Failure Injection Testing:</strong> Validate rollback behavior with controlled test deployments.</li>
  <li><strong>Multi-stage Docker Build:</strong> Optimized production images.</li>
  <li><strong>Secure AWS Access:</strong> Uses OIDC GitHub Actions authentication (no hardcoded secrets).</li>
</ul>

<h2>Architecture Overview</h2>
<pre>
GitHub Actions CI/CD
        |
        v
  Build & Push Docker Image -> Amazon ECR
        |
        v
   ECS Task Definition Update
        |
        v
   CodeDeploy Blue/Green Deployment
        |
        v
Application Load Balancer routes traffic to healthy environment
        |
        v
CloudWatch Alarms monitor health and trigger rollbacks
</pre>

<h2>Repository Structure</h2>
<pre>
.
├── app/                      # Application source code
├── docker-files/             # Dockerfiles and container build scripts
│   └── app/multistage/Dockerfile
├── .github/workflows/        # GitHub Actions CI/CD workflow
│   └── bluegreencodedeploy.yml
├── appspec.yaml              # CodeDeploy deployment specification
├── README.md                 # Project documentation
└── docker-compose.yml        # Optional local dev environment
</pre>

<h2>CI/CD Workflow</h2>
<ol>
  <li>Checkout repository</li>
  <li>Authenticate to AWS using OIDC GitHub Actions role</li>
  <li>Build multi-stage Docker image and tag with commit SHA</li>
  <li>Push image to Amazon ECR</li>
  <li>Pull current ECS task definition and update image</li>
  <li>Register new task definition revision</li>
  <li>Prepare <code>appspec.yaml</code> with updated task definition</li>
  <li>Upload <code>appspec.yaml</code> to S3</li>
  <li>Trigger CodeDeploy Blue/Green deployment</li>
</ol>
<blockquote>Note: All AWS account IDs, IAM ARNs, and S3 bucket names are replaced with placeholders for public repositories.</blockquote>

<h2>Key Learnings / Best Practices</h2>
<ul>
  <li>Gradual traffic shifting and pre-traffic health checks prevent downtime.</li>
  <li>CloudWatch alarms provide early detection of deployment issues.</li>
  <li>Controlled failure testing ensures rollback procedures are effective.</li>
  <li>ECS task definition versioning enables immutable, auditable deployments.</li>
  <li>GitHub OIDC + IAM role assumption avoids hardcoding AWS credentials.</li>
</ul>

<h2>Technologies Used</h2>
<ul>
  <li>AWS ECS (Fargate)</li>
  <li>AWS CodeDeploy</li>
  <li>Application Load Balancer (ALB)</li>
  <li>Amazon ECR</li>
  <li>CloudWatch + SNS</li>
  <li>GitHub Actions</li>
  <li>Docker (multi-stage builds)</li>
  <li>Bash / jq automation scripts</li>
</ul>

<h2>Notes for Public Repos</h2>
<ul>
  <li>Replace any AWS account IDs, IAM roles, and bucket names with placeholders.</li>
  <li>Do not upload production secrets or <code>.env</code> files.</li>
  <li>Include Dockerfiles, workflow YAMLs, and source code for reproducibility.</li>
</ul>

</body>
</html>
