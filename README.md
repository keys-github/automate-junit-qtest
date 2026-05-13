## Acknowledgments

Before diving into the details of this project, we want to extend our acknowledgment to the [shell-agent-samples](https://github.com/sanjayjohn/shell-agent-samples/tree/master/AutomationHostExamples/automateJunit) repository and its contributors. The code and concepts from this repository have significantly inspired and contributed to the development of this JUnit with Maven Build Automation tool. 

# JUnit with Maven Build Automation — TestMu AI (Formerly LambdaTest)


## Overview
This example illustrates how to use the Shell Script Automation Host Feature to pull tests from GitHub, run an Apache Maven build of JUnit tests, parse the results, and automatically upload the test results to qTest Manager. After the initial upload, the script allows the user to schedule certain tests from qTest Manager, rerun selected tests, and update only those results on qTest.


## Prerequisites
1) Install Python 3.6 from [https://www.python.org/downloads/](https://www.python.org/downloads/)

2) Install Apache Maven from [https://maven.apache.org](https://maven.apache.org/)

3) Install Git for the Command Line from [https://git-scm.com/download/](https://git-scm.com/download/)

### Tips for Set Up
Windows:

Before running the automation host script ensure that all environmental variables are set up correctly, specifically that the PATH variable has been updated for Python, Maven, and Git

Mac:

Use Homebrew to install Python and Maven. Steps for installing Homebrew can be found at [https://brew.sh](https://brew.sh)

After installing Homebrew run this following command to get Python3:

`brew install python3`

Enter the following command to get Apache Maven

`brew install maven`

### From Terminal (Mac) or Command Prompt (Windows)
1. Make sure pip was installed correctly with python on your machine by running the following command. It should output the pip version:

 `pip --version`

 Note: pip3 will work as well. Try `pip3 --version`

2. If pip is not installed, run the following command to install pip:

 `python -m -ensurepip --default-pip`

More information about downloading pip can be found at [https://packaging.python.org/tutorials/installing-packages/](https://packaging.python.org/tutorials/installing-packages/)

3. After you have ensured pip is installed, run the following commands individually:

`pip install requests`

`pip install beautifulsoup4`

`pip install lxml`

`pip install pybase64`

Note: If using pip3 run commands with pip3 instead e.g. `pip3 install requests`

These commands will install the necessary modules required to run the python scripts. The modules are used to send requests to the API, parse xml documents, and upload files to qTest.



## Update Configuration File
**git\_url:** The shell script uses the url to clone a repository and send pull requests everytime it runs if -g input is used

**local\_repository:** The folder containing the test cases. The shell script will use this to know where to run the maven build. Make sure to place this folder in the same directory as the python and shell scripts.

**qtest\_api\_token:** The token used to authorize the connection to qTest Manager

**qtest\_url:** The personal url that is used to access QASymphony API

![](../automateJunit/images/config.json.png)

Open the conf.json file and update with your personal information. Enter your own qTest URL and API Token found in the qTest Manager Environment.

For this example we will be pulling a JUnit Sample Test [https://github.com/LambdaTest/junit-qtest-sample](https://github.com/LambdaTest/junit-qtest-sample). For the git url and local repository use the information shown below in the example configuration file. Make sure to use your own api token and url or the demo will not work.

![](./automateJunit/images/config.json.png)


## Set Up Automation
### qTest Host

### 1. Download and Copy the File
- **Download** the required file from the following link:
  [Download link](https://support-hub.tricentis.com/open?sys_kb_id=194a54eedb4f5c181ea7bb13f3961950&id=kb_article_view&number=KB0015571)
  
  The file is named `agentctl-[version]-mac-x64-full.tgz`.

- **Copy** the downloaded file to your installation directory. Example:

    ```bash
    cp agentctl-[version]-mac-x64-full.tgz /usr/local/agentctl-[version]-mac-x64-full.tgz
    ```

2. **Extract the Downloaded File**
   - After downloading, extract the qTest Host file.

3. **Start the Agent**
   - Navigate to the directory where you extracted the files:
     ```
     cd /path/to/agentctl-[version]
     ```
   - Start the agent with the following command:
     ```
     ./agentctl start
     ```

4. **Open Command Line (CMD)**
   - Run this command:
     ```
     agentctl.bat start
     ```

### Accessing the Host

- **Case 1:** If the command runs successfully, open your local browser and go to:


## Navigate to the Host
```
http://localhost:6789
```

- **Case 2:** If you encounter an error like `port 6789 {6789}: could not start`, follow these steps:
1. Open the directory of qTest Automation Host.
2. Locate and open `agent.config`.
3. Change the port number as needed.

5. **Successful Start**
 - Once the configuration is correct, you should see a message indicating the port has started successfully.

6. **Final Step**
 - Run this in your browser according to your configured port number:
   ```
   http://localhost:6789
   ```
 for more detail follow : https://documentation.tricentis.com/qtest/od/en/content/launch/automation_host/installation/qtest_automation_host_2.x_installation_guide_on_windows.htm

1. Navigate to your Automation Host

 ![](./automateJunit/images/automationhosthome.png)

2.    Add a new agent and fill out the path directory and enter the script into the kick off script field

![](./automateJunit/images/add.png)
 
**Agent Name:** Name

**qTest Manager Project:** Choose your project

**Agent Type:** Choose Shell Agent

**Directory:** The directory containing your scripts and shell agent (Directory where the scripts were cloned)

**Set Allocated Execution Time:** Amount of time you expect the script to take to execute in minutes

**Kick-off scripts:** The file path to your shell script. This shell scripts takes in two inputs, one for using git and the second for updating your current test cycle.

### Shell Script Inputs
**-g**    Uses GitHub to clone a test case repository and send pull requests every time shell script is run

**-u** Updates an existing test cycle or create a new test cycle if first test run

**-b <"branch name">** If the Github parameter is used, this input allows the user to clone the repository and make a branch that they specify with the input.

To run the shell agent without using GitHub or without updating the existing test cycle do not include these parameters in the kick off scripts section. An example of not using -g is used in the JMeter Automation example.



## For Mac Users (Use run.sh)
 ![](./automateJunit/images/agentmac.png)

Note for Mac Users: Make sure the shell script is executable by running the command shown below in the shell scripts directory:

`chmod +x run.sh`

This command gives the shell script permission to run.



## For Windows Users (Use run.bat)
![](./automateJunit/images/agentwin.png)
 

## Running Shell Script
Start the shell script by pressing `kick-off shell scripts now` button under the action text field, which will upload all of the tests cases to qTest

![](./automateJunit/images/runscript.png)
 

## Scheduling Tests
1.  Login into qTest Manager, go to the Test Execution tab, and there should be a test cycle under your project called &quot;Junit Automated Tests&quot;

 ![](./automateJunit/images/junitcycle.png)

2. Click on the test cycle and it should show all of the tests that were run through the maven build and their statuses.

![](./automateJunit/images/testcycle.png)

**Tips for Maven Build:**

In the pom.xml file, make sure that maven-surefire-plugin version is up to date. The shell script runs a maven test command to run single tests that will only work on surefire-plugin versions 2.19.1 and above.


## 🚀 LambdaTest is Now TestMu AI

👋 Welcome to TestMu AI, the next evolution of LambdaTest. As of January 2026, [LambdaTest is Now TestMu AI](https://www.testmuai.com/lambdatest-is-now-testmuai/) - we have evolved from a cross-browser testing cloud into a unified, AI-native quality engineering platform designed for the modern DevOps era.

Whether you have been part of the LambdaTest community for years or are just discovering TestMu AI, our mission remains the same: to help you ship faster with high-scale test execution, autonomous testing, and deep quality analytics.

### 🔄 Our Rebrand Journey

In 2017, we introduced LambdaTest with a clear mission: to become the world's most trusted cloud testing platform. We built a scalable, high-performance test cloud that eliminated flakiness, improved developer feedback cycles, and accelerated release velocity for teams worldwide.

As LambdaTest grew, we expanded the platform into Test Intelligence, Visual Regression Testing, Accessibility Testing, API Testing, and Performance Testing, covering the entire testing lifecycle. These capabilities enabled teams to test any stack, on any technology, at enterprise scale.

Over time, we rebuilt the architecture to be AI-native from the ground up. What began as LambdaTest's high-performance testing cloud has now evolved into TestMu AI, an AI-native, multi-agent platform redefining modern quality engineering.

We chose the name TestMu AI to reflect our shift towards intelligent, autonomous testing. While our identity has changed, our core technology and commitment to the testing community stay the same.

👉 Find [LambdaTest's New Home](https://www.testmuai.com/).

### 🔭 Explore TestMu AI

The same infrastructure LambdaTest customers relied on, now delivered through autonomous AI agents.

- [KaneAI](https://www.testmuai.com/kane-ai/)
- [Agent-to-Agent Testing](https://www.testmuai.com/agent-to-agent-testing/)
- [HyperExecute](https://www.testmuai.com/hyperexecute/)
- [Real Device Cloud](https://www.testmuai.com/real-device-cloud/)
- [Pricing](https://www.testmuai.com/pricing/)
- [Documentation](https://www.testmuai.com/support/docs/)