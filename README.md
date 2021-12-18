## Installing SonarQube for python on wsl/windows

### Installing docker desktop

You can use [this link](https://docs.docker.com/desktop/windows/install/) to download latest version of docker desktop. Remember to check **Install required Windows components for WSL 2** while installing.

### Installing SonarQube image in docker
After installign docker, you can use this command to download and install SonarQube image on windows:
```markdown
docker run -d  -p 9000:9000 -p 9092:9092 sonarqube
```

For more details see [How I configured SonarQube for Python code analysis with Jenkins and Docker](https://dev.to/mmphego/how-i-configured-sonarqube-for-python-code-analysis-with-jenkins-and-docker-28fm).

### Installing SonarQube image in docker

Next step is to install sonar-scanner on WSL 2 to scan your code and check rules. You can download and install sonar-scanner accoring to this [guidline](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/).
Here we may install sonar-scanner directly via sonar-scanner-cli-4.6.2.2472-linux.zip file.

### [To run SonarScanner from the zip file, follow these steps](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/#header-2):

 1. Expand the downloaded file into the directory of your choice. We'll refer to it as $install_directory in the next steps. You may use **unzip** package from apt.
 2. Update the global settings to point to your SonarQube server by editing **$install_directory/conf/sonar-scanner.properties**:
    ```markdown
    #----- Default SonarQube server
    #sonar.host.url=http://localhost:9000
    ```

 3. Add the **$install_directory/bin** directory to your path. To do this permanently you can edit this following file:
    ```markdown
    sudo nano ~/.bash_profile
    ```
    And then adding this line at the end of it:
    ```markdown
    export PATH="$PATH:$install_directory/bin"
    ```
 4. Verify your installation by opening a new shell and executing the command sonar-scanner -h (sonar-scanner.bat -h on Windows). Don't forget to open a new terminal, to update the Path variable.
 5. 
### Scanning project with sonar-scanner

In you root directory of you project at the following file:
```markdown
# Something like your project name.
sonar.projectKey=coa_project
# folders and files to be excluded
sonar.exclusions=env/**,env/*, static/**, static/*, .git/**, .git/*, log/* 
# Here I specified for scanner to scan only python files
sonar.inclusions=**/*.py
sonar.scm.provider=git
# Token file which you may create in sonarqube clinet
sonar.login={{YOUR_SONARQUBE_TOKEN}}
sonar.sources=. 
sonar.scm.disabled=True
sonar.python.version=3
```
To create a sonarqube token just head to sonrqube server url, which you previously installed with docker and do as followes:
**Top right accoutn button** -> **My Account** -> **Security Tab** -> **Generate a new token**

After that sonar-scanner configration is done and you should be able to scan your code via **sonar-scanner** command in your project root directory. It may take a while and after that you should wait for it's processing task in **background task** in sonarqube server. After that head to your sonarqube url again, click on the projectkey appeared in projects tab. That's it.
