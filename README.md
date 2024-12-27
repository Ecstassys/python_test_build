# AWS CodeBuild - Python Hello World Application

This guide demonstrates how to set up **AWS CodeBuild** to build and execute a simple "Hello, World!" Python application hosted on GitHub. It includes steps to create a Python project on an EC2 instance, push it to GitHub, and configure AWS CodeBuild for continuous integration and building.

## Prerequisites

Before you begin, ensure you have the following:

- An **AWS account** with necessary permissions.
- A **GitHub account** and repository.
- An **EC2 instance** for creating the Python project.
- **AWS CLI** installed and configured on your local machine.
  
## Setup Instructions

### 1. **Create Python Project on EC2 Instance**

#### 1.1 SSH into Your EC2 Instance

1. Connect to your EC2 instance using SSH. Here's an example command:

    ```bash
    ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
    ```

#### 1.2 Create a Python Folder and Python File

1. Navigate to the working directory where you will create your Python project:

    ```bash
    mkdir ~/python_test_project
    cd ~/python_test_project
    ```

2. Create the Python file:

    ```bash
    touch hello_world.py
    ```

3. Write a simple Python script inside `hello_world.py`. For example:

    ```python
    # hello_world.py
    print("Hello, World!")
    ```

4. Initialize a Git repository:

    ```bash
    git init
    ```

5. Create a `.gitignore` file to ignore unwanted files:

    ```bash
    touch .gitignore
    echo "*.pyc" > .gitignore
    echo "__pycache__/" >> .gitignore
    ```

6. Commit the changes to the Git repository:

    ```bash
    git add .
    git commit -m "Initial commit for Python Hello World project"
    ```

---

### 2. **Set Up GitHub Repository**

#### 2.1 Create a New GitHub Repository

1. Log in to your GitHub account and click the **New repository** button.
2. Give your repository a name (e.g., `python_test_build`).
3. Click **Create repository**.

#### 2.2 Connect EC2 to GitHub

1. In your EC2 instance, add your GitHub repository as a remote:

    ```bash
    git remote add origin https://github.com/YourUsername/python_test_build.git
    ```

2. Push your code to GitHub:

    ```bash
    git push -u origin master
    ```

    > **Note**: If GitHub asks for your credentials, authenticate using your personal access token (PAT) instead of your GitHub password.

---

### 3. **Set Up AWS CodeBuild Project**

#### 3.1 Create a `buildspec.yml` File

AWS CodeBuild uses a `buildspec.yml` file to define the build commands for your project. Create this file in the root of your Python project on EC2 (the same directory as `hello_world.py`).

Here’s a simple `buildspec.yml` file for a Python project:

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.8  # Specify the Python version you want to use
    commands:
      - echo Installing dependencies...
      - pip install -r requirements.txt  # If you have dependencies, otherwise ignore this
  build:
    commands:
      - echo Build started on $(date)
      - python hello_world.py  # Run the Python script
  post_build:
    commands:
      - echo Build completed on $(date)

artifacts:
  files:
    - hello_world.py  # Save your Python file as an artifact if needed
