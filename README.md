<h1>ECS Blue/Green Deployment with Automated CI/CD</h1>
<p>
Built a <strong>resilient deployment pipeline on AWS</strong> enabling safe and controlled application releases.
</p>
<h2>🚀 Project Overview</h2>
<p>
This project demonstrates a <strong>production-grade DevOps pipeline</strong> for a containerized Java application using:
</p>
<ul>
  <li><strong>AWS ECS (Fargate)</strong></li>
  <li><strong>CodeDeploy (Blue/Green deployments)</strong></li>
  <li><strong>GitHub Actions (CI/CD)</strong></li>
  <li><strong>Terraform (Infrastructure as Code)</strong></li>
</ul>

<p>
The solution focuses on <strong>zero-downtime deployments</strong>, <strong>automated rollback</strong>, 
and <strong>fully reproducible infrastructure</strong>.
</p>

<hr/>

<h2>✨ Key Features</h2>
<ul>
  <li><strong>Infrastructure as Code:</strong> Full AWS environment provisioned with Terraform (VPC, ECS, ALB, IAM).</li>
  <li><strong>Modular Terraform: </strong>Structured Terraform using reusable modules improving maintainability and enabling environment reuse.</li>
  <li><strong>Blue/Green Deployment:</strong> Zero-downtime deployments using CodeDeploy traffic shifting.</li>
  <li><strong>CI/CD Automation:</strong> GitHub Actions pipeline from build to deployment.</li>
  <li><strong>Security & Quality:</strong> Trivy vulnerability scanning and SonarQube analysis.</li>
  <li><strong>Automated Rollback:</strong> CloudWatch alarms trigger rollback on failure.</li>
  <li><strong>Failure Testing:</strong> Simulated failures to validate resilience.</li>
  <li><strong>Multi-stage Docker Builds:</strong> Optimized production images.</li>
  <li><strong>Secure Authentication:</strong> GitHub OIDC (no hardcoded AWS credentials).</li>
</ul>

<hr/>

<h2>🏗 Architecture Overview</h2>
<p><strong>Provisioned entirely using Terraform:</strong></p>
<ul>
  <li>VPC with public/private subnets</li>
  <li>Application Load Balancer (ALB)</li>
  <li>ECS Cluster (Fargate)</li>
  <li>IAM roles and policies</li>
  <li>Security groups and networking</li>
</ul>
<hr>


<h2>🚀 Architecture Evolution</h2>

<h3>v1 – Manual ECS Deployment</h3>
<ul>
  <li>Manually created ECS cluster, services, and task definitions</li>
  <li>Built and pushed Docker images to ECR manually</li>
  <li>No CI/CD pipeline or deployment automation</li>
</ul>
<p><b>Problem:</b> Slow (~20 min deployments), error-prone, and not reproducible</p>

<h3>v2 – Infrastructure as Code (Terraform)</h3>
<ul>
  <li>Introduced Terraform to provision ECS, ALB, networking, and IAM</li>
  <li>Modularized infrastructure for reuse and maintainability</li>
</ul>
<p><b>Impact:</b> Reproducible environments and reduced configuration drift</p>

<h3>v3 – CI/CD Automation (GitHub Actions + OIDC)</h3>
<ul>
  <li>Implemented GitHub Actions pipeline</li>
  <li>Used OIDC for secure, keyless AWS authentication</li>
  <li>Automated build, scan (Trivy + SonarQube), and push to ECR</li>
</ul>
<p><b>Impact:</b> Fully automated build pipeline and reduced manual intervention</p>

<h3>v4 – Deployment Automation (ECS + CodeDeploy)</h3>
<ul>
  <li>Integrated CodeDeploy with ECS for Blue/Green deployments</li>
  <li>Implemented traffic shifting via ALB</li>
  <li>Configured CloudWatch alarms for automated rollback</li>
</ul>
<p><b>Impact:</b> Zero-downtime deployments with safer release strategy</p>

<h3>📊 Final Outcome</h3>
<ul>
  <li>Deployment time reduced from <b>~20 minutes → ~4 minutes</b></li>
  <li>Fully automated CI/CD pipeline</li>
  <li>Safer deployments with rollback and traffic control</li>
</ul>

<h3>🔍 Key Limitation & Next Step</h3>
<ul>
  <li>Observed that rollback strategy only reverted to the <b>previous deployment</b>, not the last known stable version</li>
  <li>This introduced risk if multiple faulty deployments occurred consecutively</li>
</ul>
<p>
<b>Insight:</b> More advanced deployment control and state management were needed for complex systems
</p>

<p>
<b>Next Step:</b> Explored Kubernetes-based solutions to achieve greater flexibility in rollout strategies and deployment control
</p>

<hr>
<h2>🏗 Architecture Diagram </h2>

<img width="512" height="7518" src="https://github.com/user-attachments/assets/640fdd84-5944-4a59-a13d-22fa29d6c53b" />

<hr/>

<h2>📁 Repository Structure</h2>
<pre>
.
├── app/
├── docker-files/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── modules/
├── .github/workflows/
├── appspec.yaml
├── docker-compose.yml
└── README.md
</pre>

<hr/>

<h2>⚙️ Infrastructure Provisioning (Terraform)</h2>
<ol>
  <li>Define infrastructure using Terraform</li>
  <li>Provision VPC, subnets, and networking</li>
  <li>Deploy ECS Fargate cluster and services</li>
  <li>Configure ALB and target groups</li>
  <li>Create IAM roles for ECS, CodeDeploy, and OIDC</li>
</ol>

<p><strong>Benefits:</strong></p>
<ul>
  <li>Reproducible environments</li>
  <li>Version-controlled infrastructure</li>
  <li>Faster recovery and debugging</li>
  <li>Production-aligned DevOps practices</li>
</ul>

<hr/>

<h2>🔄 CI/CD Workflow</h2>
<ol>
  <li>Checkout repository</li>
  <li>Authenticate to AWS via OIDC</li>
  <li>Run tests + Trivy + SonarQube</li>
  <li>Build Docker image (tagged with commit SHA)</li>
  <li>Push image to Amazon ECR</li>
  <li>Update ECS task definition</li>
  <li>Register new revision</li>
  <li>Generate <code>appspec.yaml</code></li>
  <li>Upload to S3</li>
  <li>Trigger CodeDeploy Blue/Green deployment</li>
</ol>

<hr/>

<h2>🔗 Key Resources & Documentation</h2>
<ul>
  <li><a href="https://docs.aws.amazon.com/ecs/">AWS ECS (Fargate)</a></li>
  <li><a href="https://docs.aws.amazon.com/codedeploy/">AWS CodeDeploy</a></li>
  <li><a href="https://registry.terraform.io/providers/hashicorp/aws/latest/docs">Terraform AWS Provider</a></li>
  <li><a href="https://docs.github.com/en/actions">GitHub Actions</a></li>
  <li><a href="https://aquasecurity.github.io/trivy/">Trivy Scanner</a></li>
  <li><a href="https://docs.sonarqube.org/">SonarQube</a></li>
  <li><a href="https://docs.docker.com/">Docker</a></li>
</ul>

<hr/>

<h2>⚠️ Failures &amp; Lessons Learned</h2>

<h3>Issue</h3>
<p>
Stopping a deployment mid-traffic shift caused subsequent deployments to fail.
</p>

<h3>Root Cause</h3>
<ul>
  <li>ALB target group weights left in inconsistent state</li>
  <li>CodeDeploy requires baseline: 100% Blue / 0% Green</li>
</ul>

<h3>Resolution</h3>
<ul>
  <li>Inspected ALB listener rules and identified incorrect traffic weights</li>
  <li>Manually reset weights to restore service stability (100% Blue / 0% Green)</li>
  <li>Reconciled infrastructure using Terraform to ensure state consistency</li>
  <li>Successfully re-triggered deployment</li>
</ul>

<h3>Key Learnings</h3>
<ul>
  <li>Maintain deployment state consistency</li>
  <li>Avoid interrupting live deployments</li>
  <li>Terraform enables quick environment recovery</li>
  <li>Failure testing improves reliability</li>
</ul>

<hr/>

<h2>🛠 Technologies Used</h2>
<ul>
  <li>Terraform</li>
  <li>AWS ECS (Fargate)</li>
  <li>AWS CodeDeploy</li>
  <li>Application Load Balancer</li>
  <li>Amazon ECR</li>
  <li>CloudWatch + SNS</li>
  <li>GitHub Actions</li>
  <li>Docker</li>
  <li>Trivy + SonarQube</li>
</ul>

<hr/>

<h2>📸 Screenshots Of My Work</h2>

<ul>
  <li>
    <p><strong>GitHub Actions Workflow</strong></p>
    <img width="630" height="245" src="https://github.com/user-attachments/assets/de053663-9ad9-4ce0-bcfb-a9eda56c342a" />
  </li>

  <li>
    <p><strong>CodeDeploy Blue/Green Traffic Shift</strong></p>
    <img width="744" height="311" src="https://github.com/user-attachments/assets/1ef441e0-701a-47ac-8300-37e976ed6189" />
  </li>

  <li>
    <p><strong>Rollback via CloudWatch Alarms</strong></p>
    <img width="584" height="425" src="https://github.com/user-attachments/assets/eb1fc54d-6932-48a4-9153-3d08072bb864" />
  </li>

  <li>
    <p><strong>Multi-Stage Docker Build</strong></p>
    <img width="1094" height="352" src="https://github.com/user-attachments/assets/08556c63-e5f2-4367-a7f7-8134a65f2dd7" />
  </li>
</ul>

<hr/>

<h2>📌 Notes for Public Repositories</h2>
<ul>
  <li>Replace AWS account IDs, IAM roles, and bucket names</li>
  <li>Do not commit secrets or <code>.env</code> files</li>
  <li>Include Terraform for full reproducibility</li>
</ul>

<hr/>

<h2>💡 Portfolio Impact</h2>
<p>
This project showcases:
</p>
<ul>
  <li>Production-grade CI/CD pipeline design</li>
  <li>Zero-downtime deployment strategies</li>
  <li>Infrastructure automation with Terraform</li>
  <li>Cloud-native security and reliability practices</li>
</ul>
