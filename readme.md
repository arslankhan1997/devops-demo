DevOps Demo — Simple Python App on EC2
A minimal Flask web app with a complete DevOps pipeline: Docker + GitHub Actions CI/CD → AWS EC2.

Project Structure
devops-demo/
├── app.py                        # Flask application
├── requirements.txt              # Python dependencies
├── Dockerfile                    # Container definition
├── test_app.py                   # Basic tests
└── .github/
    └── workflows/
        └── deploy.yml            # CI/CD pipeline

Architecture
GitHub Push
    │
    ▼
GitHub Actions
    ├── Run Tests (pytest)
    ├── Build Docker Image
    └── SSH into EC2 → docker run
                          │
                          ▼
                    EC2 Instance
                    (port 80 → Flask:5000)

Tools & Technologies
ToolPurposePython / FlaskWeb applicationDockerContainerizationGitHub ActionsCI/CD automationAWS EC2Cloud hosting (Ubuntu)pytestTesting

Local Development
bash# Run directly
pip install flask
python app.py

# Run with Docker
docker build -t devops-demo .
docker run -p 5000:5000 devops-demo

# Run tests
pip install pytest flask
pytest test_app.py -v
App runs at: http://localhost:5000

EC2 Setup (One-time)

Launch EC2 — Ubuntu 22.04, t2.micro (free tier), open ports 22 and 80 in Security Group.
SSH into EC2 and install Docker:

bashssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>

sudo apt update && sudo apt install -y docker.io
sudo usermod -aG docker ubuntu
newgrp docker

Add GitHub Secrets (Settings → Secrets → Actions):

SecretValueEC2_HOSTYour EC2 public IPEC2_USERubuntuEC2_KEYContents of your .pem private key file

CI/CD Pipeline (GitHub Actions)
On every push to main, the pipeline automatically:

Runs tests with pytest
Builds the Docker image
SSHs into EC2 and redeploys the container

No manual steps needed after initial setup.

Deployment Steps (Manual)
If you want to deploy manually on EC2:
bashgit clone https://github.com/YOUR_USERNAME/devops-demo.git
cd devops-demo
docker build -t devops-demo .
docker run -d --name devops-demo -p 80:5000 devops-demo
Visit: http://<EC2_PUBLIC_IP>
