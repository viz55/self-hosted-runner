# 🚀 GitHub Actions — Self-Hosted Runner on AWS EC2

![Workflow Status](https://github.com/viz55/self-hosted-runner/actions/workflows/action_file.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.8%20%7C%203.9-blue?logo=python)
![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![License](https://img.shields.io/github/license/viz55/self-hosted-runner)

A CI/CD pipeline that runs automated Python tests on a **self-hosted GitHub Actions runner** hosted on an AWS EC2 instance — instead of relying on GitHub's own (shared, rate-limited) hosted runners.

---

## 📌 Why this project matters

GitHub-hosted runners are convenient but have limits: fixed specs, limited minutes on free tiers, and no control over the environment. This project shows how to stand up your **own compute** for CI, which is exactly what companies do when they need custom hardware, private network access, or to cut CI costs at scale.

## 🧱 Architecture

```
Push to GitHub  ──▶  GitHub Actions Workflow Triggered
                             │
                             ▼
                  Self-Hosted Runner (AWS EC2)
                             │
                             ▼
                Installs deps → Runs pytest (3.8 & 3.9)
                             │
                             ▼
              ✅ / ❌ Status reported back to GitHub
```

## ⚙️ What the workflow does

- Triggers on every push to the repository
- Runs on a **self-hosted runner** (your own AWS EC2 instance)
- Tests the Python codebase with `pytest` across **Python 3.8 and 3.9**
- Workflow file: [`.github/workflows/action_file.yml`](.github/workflows/action_file.yml)

## 🛠️ Setup: Connecting an EC2 Runner to GitHub

### 1. Launch an EC2 instance
- AWS Console → EC2 → Launch Instance
- AMI: Amazon Linux or Ubuntu
- Security group: allow inbound SSH (22), HTTP (80), HTTPS (443)

### 2. SSH into the instance
```bash
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```

### 3. Update packages
```bash
sudo apt update -y
```

### 4. Register the runner with GitHub
- Repo → **Settings → Actions → Runners → New self-hosted runner**
- Choose **Linux → x64**
- Run the exact commands GitHub shows you to download and configure the runner


### 5. Start the runner
```bash
./run.sh
```

<img width="751" height="576" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/dd4bf9ad-a0e3-429b-9167-8d761c1ad519" />

> ⚠️ **Important:** In `action_file.yml`, set `runs-on: self-hosted` (not `ubuntu-latest`) — otherwise your jobs will run on GitHub's own infrastructure instead of your EC2 box.

## ✅ Verifying it works

Push a change to the repo. You should see the job picked up live in your EC2 terminal running `run.sh`, and the result (pass/fail) reflected in the **Actions** tab on GitHub.

<img width="739" height="511" alt="Screenshot (37)" src="https://github.com/user-attachments/assets/4fa9a8c9-fa6e-40dc-87f3-513123625151" />


## 📈 Possible extensions
See the "what's next" ideas in the project write-up — turning this from a demo into a small piece of real infrastructure (auto-scaling runners, Docker isolation, monitoring, IaC).

## 🧰 Tech Stack
`AWS EC2` · `GitHub Actions` · `Python` · `pytest` · `Linux`

## 📄 License
MIT — see [LICENSE](LICENSE)
