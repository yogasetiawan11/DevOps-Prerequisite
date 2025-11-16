# what is owasp

OWASP Dependency Check is an open-source Software Composition Analysis (SCA) tool that proactively identifies project dependencies and verifies whether there are known Common Vulnerabilities and Exposures (CVEs) associated with them. This is crucial for managing the security risks introduced by third-party libraries and components.

# Use case with maven app
change directory where pom.xml is located 
```bash
mvn clean package
```
this command will build artifact for your app, and located in ``/target/``

Download the owasp from github
```bash
wget 
```

Run dependency check
```bash
.\bin\dependency-check.bat --out . --scan [path to jar files to be scanned]
```
``--out`` = stands for output where you can specify the path of your output of this command.
``--scan`` = specify the path of artfact that just created using ```mvn clean package``` or ``/target/``

Once this is done. it will store the output into``.html``


# Run OWASP command from jenkins
Go to Jenkins >> Manage Jenkins >> Plugins >>


<img width="1335" height="637" alt="Image" src="https://github.com/user-attachments/assets/aeb0f56a-c3a3-47d1-90f1-cd74004667ce" />

Then run OWASP script in the pipeline 
```sh
    stages {
        stage('OWASP Dependency-check') {
            steps {
                dependencyCheck additionalArguments:'-scan target/', odcInstallation 'owasp'
                     dependencyCheckPublisher pattern: '**/dependency-check-report-pom.xml'    
            }
        
    }
```

- Configure Jenkins Globally
Go to Jenkins >> Manage Jenkins >> Tools >> click "Add Dependency-Check" >> Make sure you fill the same name of "odcInstallation".
