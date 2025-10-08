# GitHub Actions — Self-Hosted Runner on AWS EC2

This project demonstrates how to set up a *self-hosted GitHub Actions runner* on an *AWS EC2 instance* to run CI/CD pipelines for a Python project.  
It runs automated tests using *python* & *pytest* every time code is pushed to the repository.

## Project Overview

This workflow:
- Uses **GitHub Actions** for CI/CD.
- Runs on a **self-hosted runner** (hosted on AWS EC2).
- Tests Python code using **pytest** on multiple versions of Python (3.8 and 3.9).

## Workflow File

The main workflow file is located at:  `.github/workflows/action_file.yml`

## Setting Up a Self-Hosted Runner on AWS EC2

**Follow these steps to connect your EC2 instance to GitHub Actions as a runner:**

*Step 1: Launch an EC2 Instance*

- Go to AWS Console → EC2 → Launch Instance
- Choose:
`Amazon Linux` or `Ubuntu`
- Configure security groups of the instance inside security option to allow SSH (port 22), HTTP (port 80) and HTTPS (port 443) as shown: -

<img width="1267" height="595" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/6d374772-d70c-42ef-97e8-2876938d1186" />

*Step 2: Connect to EC2 via SSH*

```
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```

*Step 3: Update the Packages*

```
sudo apt update -y
```

*Step 4: Register Runner with GitHub*
- Go to your **GitHub repository → Settings → Actions → Runners → New self-hosted runner**
- Choose **Linux → x64** in architecture
- Follow the commands shown in the website to configure the self-hosted runner.

*Step 5: Start the Runner*
use the command: `./run.sh` to start the runner in the server.

Once it starts, you’ll get am output like this: -

<img width="751" height="576" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/dd4bf9ad-a0e3-429b-9167-8d761c1ad519" />

## Testing the Workflow:

Make any change in the code provided inside `.github/workflows/action_file.yml` and push to GitHub

#### Note: Change the `runs-on: ubuntu-latest` to `runs-on: self-hosted` in the code and push to github before making any other changes to run the code inside self-hosted runners and not github-hosted runners.

After making a change you will be getting an Output like this in the ec2 server:

<img width="739" height="511" alt="Screenshot (37)" src="https://github.com/user-attachments/assets/4fa9a8c9-fa6e-40dc-87f3-513123625151" />

**This output ensures that the code has integrated AWS EC2 with GitHub Actions using a self-hosted runner & automated Python testing using pytest.**

