# dsol

import os
try:
    from reportlab.lib.pagesizes import letter
    from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak, Preformatted
    from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
    from reportlab.lib import colors
except ImportError:
    print("Installing required library: reportlab...")
    os.system("pip install reportlab")
    from reportlab.lib.pagesizes import letter
    from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak, Preformatted
    from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
    from reportlab.lib import colors

def create_devsecops_pdf(filename="DevSecOps_Lab_Guide.pdf"):
    doc = SimpleDocTemplate(
        filename,
        pagesize=letter,
        rightMargin=40,
        leftMargin=40,
        topMargin=40,
        bottomMargin=40
    )
    
    styles = getSampleStyleSheet()
    
    # Custom Styles for High Scannability
    title_style = ParagraphStyle(
        'DocTitle',
        parent=styles['Heading1'],
        fontSize=24,
        leading=28,
        textColor=colors.HexColor('#1A365D'),
        spaceAfter=20,
        alignment=1 # Centered
    )
    
    section_style = ParagraphStyle(
        'SectionHeader',
        parent=styles['Heading2'],
        fontSize=14,
        leading=18,
        textColor=colors.HexColor('#2B6CB0'),
        spaceBefore=12,
        spaceAfter=6,
        keepWithNext=True
    )
    
    body_style = ParagraphStyle(
        'BodyTextCustom',
        parent=styles['Normal'],
        fontSize=10,
        leading=14,
        textColor=colors.HexColor('#2D3748'),
        spaceAfter=4
    )
    
    code_style = ParagraphStyle(
        'CodeStyle',
        fontName='Helvetica-Oblique',
        fontSize=9,
        leading=12,
        textColor=colors.HexColor('#C53030'),
        backColor=colors.HexColor('#EDF2F7'),
        borderColor=colors.HexColor('#CBD5E0'),
        borderWidth=0.5,
        borderPadding=6,
        spaceBefore=4,
        spaceAfter=6
    )

    story = []
    
    # Document Title Page / Header
    story.append(Paragraph("<b>DevSecOps Lab Comprehensive Study Guide</b>", title_style))
    story.append(Paragraph("<i>A step-by-step implementation reference manual for Git, AWS, Docker, Terraform, Ansible, Jenkins, and Prometheus.</i>", body_style))
    story.append(Spacer(1, 15))
    
    labs = [
        # SECTION 1: GIT & GITHUB
        ("1. Git Basics (Local Repository)", 
         "Initialize a repository, track files, make a commit, alter history, and view logs.\n\n"
         "<b>Step 1: Initialize the repository</b>\n"
         "mkdir DevSecOpsLab && cd DevSecOpsLab\ngit init\n\n"
         "<b>Step 2: Create two files</b>\n"
         "echo 'File 1 content' > file1.txt\necho 'File 2 content' > file2.txt\n\n"
         "<b>Step 3: Stage and commit the files</b>\n"
         "git add .\ngit commit -m 'Initial commit with file1 and file2'\n\n"
         "<b>Step 4: Modify a file and view history</b>\n"
         "echo 'Updated content' >> file1.txt\ngit add file1.txt\ngit commit -m 'Modify file1'\ngit log --oneline"),
         
        ("2. GitHub Synchronization", 
         "Link your local project folder up to a remote host cloud server.\n\n"
         "<b>Step 1: Create a remote GitHub repository</b>\n"
         "Log into GitHub -> Click 'New' -> Name it 'DevSecOpsLab' -> Keep it empty -> Click 'Create'.\n\n"
         "<b>Step 2: Link and push local repository</b>\n"
         "git remote add origin https://github.com\ngit branch -M main\ngit push -u origin main\n\n"
         "<b>Step 3: Verify</b>\n"
         "Refresh your GitHub repository browser page to see your committed text files."),
         
        ("3. Git Branching & Collaboration", 
         "Isolate your new features safely away from production lines inside feature branches.\n\n"
         "<b>Step 1: Clone the remote repository</b>\n"
         "git clone https://github.com\ncd DevSecOpsLab\n\n"
         "<b>Step 2: Create and switch to a feature branch</b>\n"
         "git checkout -b feature\n\n"
         "<b>Step 3: Make changes and push to GitHub</b>\n"
         "echo 'New Feature Data' > feature.txt\ngit add feature.txt\ngit commit -m 'Add feature branch files'\ngit push origin feature"),

        # SECTION 2: AWS EC2
        ("4. Launch & Connect to Ubuntu EC2", 
         "Deploy a basic Linux virtual machine instance up inside your cloud account provider.\n\n"
         "<b>Step 1: Launch via AWS Management Console</b>\n"
         "Navigate to EC2 -> Click 'Launch Instance' -> Select 'Ubuntu Server 22.04 LTS' -> Choose 't2.micro' -> Create or select an existing Key Pair (.pem) -> Launch Instance.\n\n"
         "<b>Step 2: Fix local key permissions (Linux/Mac)</b>\n"
         "chmod 400 your-key.pem\n\n"
         "<b>Step 3: Connect to the instance using SSH</b>\n"
         "ssh -i 'your-key.pem' ubuntu@your-ec2-public-ip"),
         
        ("5. Configure Security Groups", 
         "Set up cloud firewall rules protecting port interfaces on active servers.\n\n"
         "<b>Step 1: Create a Security Group</b>\n"
         "EC2 Dashboard -> Network & Security -> Security Groups -> Click 'Create Security Group'.\n\n"
         "<b>Step 2: Add Inbound Rules</b>\n"
         "- Rule 1: Type: SSH | Port: 22 | Source: My IP\n- Rule 2: Type: HTTP | Port: 80 | Source: Anywhere (0.0.0.0/0)\n\n"
         "<b>Step 3: Attach Security Group to EC2</b>\n"
         "Right-click your running EC2 Instance -> Security -> Change Security Groups -> Select your newly created Security Group -> Save."),
         
        ("6. Host a Simple HTML Page", 
         "Turn your basic Linux compute resource node into an active serving web server.\n\n"
         "<b>Step 1: Connect to EC2 and install Apache web server</b>\n"
         "sudo apt update && sudo apt install apache2 -y\n\n"
         "<b>Step 2: Create a custom landing web page index document</b>\n"
         "echo '<h1>Welcome to DevSecOps Lab Web Server</h1>' | sudo tee /var/www/html/index.html\n\n"
         "<b>Step 3: Verify</b>\n"
         "Open a web browser and navigate to http://YOUR_EC2_PUBLIC_IP"),

        # SECTION 3: DOCKER
        ("7. Docker Installation & Nginx Container", 
         "Containerize simple web applications locally to skip manually downloading web software.\n\n"
         "<b>Step 1: Update packages and install Docker engine</b>\n"
         "sudo apt update && sudo apt install docker.io -y\nsudo systemctl start docker && sudo systemctl enable docker\nsudo usermod -aG docker $USER && newgrp docker\n\n"
         "<b>Step 2: Run a background detached Nginx container mapped to port 80</b>\n"
         "docker run -d -p 80:80 --name web-nginx nginx\n\n"
         "<b>Step 3: Verify</b>\n"
         "Access http://YOUR_EC2_PUBLIC_IP to see the default Nginx welcome splash page."),
         
        ("8. Docker Image Interactivity", 
         "Pull base environments, dive directly inside them, and execute instructions.\n\n"
         "<b>Step 1: Pull official Ubuntu image and run an interactive terminal session</b>\n"
         "docker run -it ubuntu /bin/bash\n\n"
         "<b>Step 2: Execute command inside the isolated container layer</b>\n"
         "apt update && apt install neofetch -y && neofetch\n\n"
         "<b>Step 3: Exit container engine space</b>\n"
         "exit"),
         
        ("9. Dockerfile for Flask App", 
         "Package unique python application scripts into predictable immutable artifacts.\n\n"
         "<b>Step 1: Create a basic application file (app.py)</b>\n"
         "from flask import Flask\napp = Flask(__name__)\n@app.route('/')\ndef home(): return 'Flask App Running!'\nif __name__ == '__main__': app.run(host='0.0.0.0', port=5000)\n\n"
         "<b>Step 2: Write the Dockerfile configuration blueprint</b>\n"
         "FROM python:3.9-slim\nWORKDIR /app\nRUN pip install flask\nCOPY app.py .\nEXPOSE 5000\nCMD ['python', 'app.py']\n\n"
         "<b>Step 3: Build code image asset and launch on container engines</b>\n"
         "docker build -t my-flask-app .\ndocker run -d -p 5000:5000 --name flask-container my-flask-app"),
         
        ("10. Docker Housekeeping", 
         "Audit container states to keep staging disk allocations clean.\n\n"
         "<b>Step 1: Display active and inactive resource allocations</b>\n"
         "docker images\ndocker ps -a\n\n"
         "<b>Step 2: Clean up single specified elements</b>\n"
         "docker rm flask-container\ndocker rmi my-flask-app\n\n"
         "<b>Step 3: Wipe all stopped containers and dangling images</b>\n"
         "docker system prune -a --volumes -f"),
         
        ("11. Docker Compose Multi-Container Deployment", 
         "Orchestrate compound configurations containing python backends and database elements.\n\n"
         "<b>Step 1: Create a docker-compose.yml orchestration recipe</b>\n"
         "version: '3.8'\nservices:\n  web:\n    build: .\n    ports:\n      - '5000:5000'\n    depends_on:\n      - db\n  db:\n    image: mysql:8.0\n    environment:\n      MYSQL_ROOT_PASSWORD: secretpassword\n      MYSQL_DATABASE: labdb\n\n"
         "<b>Step 2: Spin up environment or bring down complete state</b>\n"
         "docker-compose up -d\ndocker-compose down"),

        # SECTION 4: INFRASTRUCTURE AS CODE (TERRAFORM)
        ("12. Terraform Infrastructure Provisioning", 
         "Write declarations to automatically spin up explicit computing setups.\n\n"
         "<b>Step 1: Create main.tf configuration manifest</b>\n"
         "provider 'aws' {\n  region = 'us-east-1'\n}\nresource 'aws_instance' 'web' {\n  ami           = 'ami-0c7217cdde317cfec'\n  instance_type = 't2.micro'\n  tags = {\n    Name = 'Terraform-EC2-Lab'\n  }\n}\n\n"
         "<b>Step 2: Initialize, execute plan, and apply the infrastructure</b>\n"
         "terraform init\nterraform plan\nterraform apply -auto-approve"),
         # SECTION 5: CONFIGURATION MANAGEMENT (ANSIBLE)
        ("13. Ansible Node Connectivity Verification","Manage distributed host control networks easily via structured inventory configurations.\n\n"
         "Step 1: Define explicit target destinations via inventory hosts file\n"
         "[managed_nodes]\nmanaged1_ip ansible_user=ubuntu ansible_ssh_private_key_file=key.pem\n\n"
         "Step 2: Fire ping test modules across registered environments\n"
         "ansible managed_nodes -m ping -i hosts"),
        ("14. Ansible Playbook for Web Server","Automate installation configurations smoothly without typing manual host setup actions.\n\n"
         "Step 1: Write a site setup playbook configuration (webserver.yml)\n"
         "- name: Install Webserver\n  hosts: managed_nodes\n  become: true\n  tasks:\n    - name: Ensure Nginx is at latest version\n      apt:\n        name: nginx\n        state: latest\n    - name: Start Nginx background service engine\n      service:\n        name: nginx\n        state: started\n\n"
         "Step 2: Execute playbook across host machines\n"
         "ansible-playbook -i hosts webserver.yml"),
        ("15. Ansible Playbook User Provisioning","Establish identical operating accounts systematically across remote compute instances.\n\n"
         "Step 1: Write user creation configurations (create_user.yml)\n"
         "- name: User Management\n  hosts: managed_nodes\n  become: true\n  tasks:\n    - name: Provision a new system account user\n      user:\n        name: devopsuser\n        shell: /bin/bash\n        groups: sudo\n\n"
         "Step 2: Execute user creation playbook\n"
         "ansible-playbook -i hosts create_user.yml"),
        ("16. Ansible Ad-hoc Commands","Check live operational telemetry items across fleets with one-off shell lines.\n\n"
         "Step 1: Fetch explicit disk storage stats instantly across node servers\n"
         "ansible managed_nodes -m shell -a 'df -h' -i hosts"),
         # SECTION 6: CI/CD (JENKINS)
         ("17. Jenkins Setup on AWS EC2","Deploy standard automation execution master frameworks on enterprise nodes.\n\n""Step 1: Install Java and Jenkins engine assets\n""sudo apt update && sudo apt install openjdk-17-jre -y\nwget -q -O - jenkins.io | sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg\necho deb [signed-by=/usr/share/keyrings/jenkins.gpg] jenkins.io binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list\nsudo apt update && sudo apt install jenkins -y\nsudo systemctl start jenkins\n\n""Step 2: Unlock Dashboard UI panel\n""Navigate to http://YOUR_EC2_PUBLIC_IP:8080 -> Run: sudo cat /var/lib/jenkins/secrets/initialAdminPassword -> Copy string output to browser to authorize."),
         ("18. Jenkins Freestyle Project","Create jobs executing low-complexity basic tasks directly through automation frames.\n\n""Step 1: Configure build steps\n""Dashboard -> New Item -> 'SystemInfoJob' -> Freestyle Project -> OK -> Build Steps -> Add Build Step -> Execute Shell.\n\n""Step 2: Write monitoring commands inside configuration prompt\n""uname -a\nfree -m\nlscpu\n\n""Step 3: Run\n""Click Save -> Click 'Build Now' -> Open Console Output history records to verify system printouts."),("19. Jenkins SCM Integration","Trigger operations directly from centralized code repositories on demand.\n\n""Step 1: Configure project resource targets\n""New Item -> 'GitHub-Freestyle' -> Freestyle Project -> Source Code Management -> Select 'Git' -> Repository URL: github.com -> Save.\n\n""Step 2: Run Manual Execution Checks\n""Click 'Build Now' to see checking steps retrieve code files into working machine zones."),("20. Declarative Jenkins Pipeline Workflow","Establish robust staged visual workflows tracking items from creation to deployments.\n\n""Step 1: Define continuous pipeline script models\n""New Item -> 'DevSecOpsPipeline' -> Select 'Pipeline' -> Under pipeline script definition area, supply:\n""pipeline {\n  agent any\n  stages {\n    stage('Clone') { steps { git 'github.com' } }\n    stage('Build') { steps { echo 'Building Project Assets...' } }\n    stage('Test') { steps { echo 'Running Quality Verification Unit Suites...' } }\n    stage('Deploy') { steps { echo 'Pushing Container Imagery to Staging Production Target...' } }\n  }\n}\n\n""Step 2: Run Job\n""Save and execute 'Build Now' to review beautiful step processing pipeline matrices visually."),
         # SECTION 7: MONITORING (PROMETHEUS)
         ("21. Monitoring Setup (Prometheus & Node Exporter)","Expose live telemetry metrics from system hardware architectures onto visual timelines.\n\n""Step 1: Set up Prometheus & Node Exporter binaries\n""sudo apt update && sudo apt install prometheus prometheus-node-exporter -y\n\n""Step 2: Verify metric emission pipelines are running cleanly\n""Prometheus Engine Dashboard target: http://YOUR_EC2_PUBLIC_IP:9090\nNode Exporter Metrics emitter interface target: http://YOUR_EC2_PUBLIC_IP:9100/metrics"),("22. Prometheus Target Configuration Tuning","Ingest scraping specifications explicitly targeting local metric endpoint providers.\n\n""Step 1: Update scraping job definitions file (/etc/prometheus/prometheus.yml)\n""scrape_configs:\n  - job_name: 'node_exporter'\n    static_configs:\n      - targets: ['localhost:9100']\n\n""Step 2: Apply file adjustments\n""sudo systemctl restart prometheus"),
         ("23. Alerting Rules Infrastructure Setup","Program system parameters to announce spikes before computing outrages occur.\n\n""Step 1: Append configuration alerting evaluation rule scripts (/etc/prometheus/alert.rules.yml)\n""groups:\n  - name: resource_alerts\n    rules:\n      - alert: HighCpuUsage\n        expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode='idle'}[5m])) * 100) > 80\n        for: 2m\n        labels:\n          severity: critical\n        annotations:\n          summary: 'Instance CPU usage high'\n\n""Step 2: Register rule paths inside main app setups and refresh systems\n""Add 'rule_files: ["alert.rules.yml"]' into /etc/prometheus/prometheus.yml and restart service."),
        # SECTION 8: SYNTHESIS CAPSTONE PROJECTS
         ("24. Capstone: Complete App Containerization Lifecycle","Merge cloud virtual provisioning targets directly with running application environments.\n\n""Step 1: Launch EC2 with Docker installed (Reference Lab 4 & Lab 7 steps)\n""Step 2: Run containerized environments directly on target system hosts\n""docker run -d -p 80:80 --name global-web nginx\n\n""Step 3: Confirm Browser Reachability\n""Open http://YOUR_EC2_PUBLIC_IP and inspect the welcome greeting message output canvas."),("25. Capstone: Distributed Container Registries (Docker Hub)","Compile web framework components, deliver to central hubs, and deploy anywhere.\n\n""Step 1: Create application files, build images, and authenticate registries\n""docker build -t your_dockerhub_username/flaskapp:v1 .\ndocker login\n\n""Step 2: Distribute image assets up to your cloud hub spaces\n""docker push your_dockerhub_username/flaskapp:v1\n\n""Step 3: Pull down image elements down on target EC2 compute nodes to launch\n""ssh -i key.pem ubuntu@ec2-ip 'docker run -d -p 5000:5000 your_dockerhub_username/flaskapp:v1'"),("26. Capstone: GitOps IaC Infrastructure Automation","Provision raw instances using code configurations, then immediately shape settings with configuration files.\n\n""Step 1: Execute Infrastructure Provisioning\n""Run terraform apply -auto-approve (Lab 12 code) to dynamically generate target cloud machine instances.\n\n""Step 2: Map new live Target IP locations inside local orchestration file stores\n""Capture newly created instance dynamic IP address and paste into Ansible hosts list.\n\n""Step 3: Implement Configuration Task Plays\n""Run ansible-playbook -i hosts webserver.yml (Lab 14 code) to instantly shape app dependencies over the newly deployed systems."),("27. Capstone: End-to-End Git-Triggered App Delivery (CI/CD)","Establish modern pipelines tracking checking entries straight into immediate web rollouts.\n\n""Step 1: Interconnect Jenkins engine with external target Git spaces (Follow Lab 17 setup guidelines)\n""Step 2: Establish robust declarative pipeline execution instructions using explicit multi-stage models:\n""pipeline {\n  agent any\n  stages {\n    stage('Fetch') { steps { git 'github.com' } }\n    stage('Compile') { steps { sh 'docker build -t app:latest .' } }\n    stage('App Rollout') { steps { sh 'docker rm -f active-app || true', 'docker run -d -p 5000:5000 --name active-app app:latest' } }\n  }\n}\n\n""Step 3: Confirm browser outputs\n""Access http://YOUR_EC2_PUBLIC_IP:5000 to verify successful automated delivery cycles."),("28. Capstone: Full Stack Telemetry Infrastructure Setup","Unify target tracking across platforms to preserve clear insight views across complete systems.\n\n""Step 1: Install full metrics exporters on active cloud host systems (Follow Lab 21 procedures)\n""Step 2: Standardize monitoring collection scraping parameters inside tracking setups\n""Confirm that scrape targets point to port 9100 inside the main prometheus configuration file.\n\n""Step 3: Track metrics inside visualization consoles\n""Open web dashboard metric search boxes and query explicit fields (e.g., node_memory_Active_bytes) to track live environment consumption graphics.")]
    
    for title, text in labs:
        story.append(Paragraph(title, section_style))
    # Split text by line to preserve code blocks cleanly vs explanatory 
        lines = text.split('\n')
        code_block = []
        in_code = False
    for line in lines:
    # Simple heuristic: lines starting with specific bash tokens or pure configuration structures 
    # go to code formatting block 
        is_cmd_line = any(line.strip().startswith(x) 
    for x in ['git ', 'mkdir ', 'cd ', 'echo ', 'sudo ', 'ssh ', 'terraform ','ansible', 'docker', 'pip ', 'apt ', 'wget ', 'FROM ', 'WORKDIR ','RUN ', 'COPY ', 'EXPOSE ', 'CMD ', 'provider ', 'resource ','version:', 'services:', 'web:', 'db:', 'pipeline ', 'agent ', 'stages ', 'stage(']) or (len(code_block) > 0 and line.startswith('  ')) or (line.strip().startswith('- name:'))
    if is_cmd_line:code_block.append(line)
    
    else:
        if code_block:story.append(Preformatted('\n'.join(code_block), code_style))
        code_block = []
        if line.strip():story.append(Paragraph(line, body_style))
        if code_block:story.append(Preformatted('\n'.join(code_block), code_style))
        story.append(Spacer(1, 8))
        
    doc.build(story)
    print(f"Success! Perfect high-density scannable manual generated: {filename}")
    if name == 'main':create_devsecops_pdf()
