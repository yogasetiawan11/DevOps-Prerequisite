# Gitleaks
Gitleaks is an open-source security tool designed to detect and prevent the accidental exposure of sensitive information—such as passwords, API keys, tokens, secret key, access key and other secrets—in Git repositories. 

It is going tell us in exact location if it contains the sensitive information.

# How to use it
There are multiple way to use gitleaks: 
You can use gitleaks on ubuntu machine, you can clone git repo and then you can scan     the repo and it will generate the report 
Without cloning you can also scan repository and generate the report
Use gitleaks inside CI/CD pipeline, “suggestion” if you use gitleaks in lower environment, when you find the sensitive data you immediately should abort or exit the pipeline, so the deployment doesn’t happen

- Download gitleaks

```sh
sudo apt install gitleaks
```

- Scan Repository
```sh
gitleaks detect --source <repo-name>
```

- Detect sensitive information and store into path with json format
```sh
gitleaks detect --source . -r report-gitleaks.json -f json 
```
You can also save into csv format just modify json syntax and format with csv

## Use whitin Jenkins Pipeline
- Install jenkins
- Because gitleaks is not available on jenkins plugin, all you need to do install it directly on machine  “sudo apt install gitleaks”

```sh
stage (‘gitleaks-detection’) {
     steps{	
       sh “gitleaks detect --source . -r report-gitleaks.json -f json”

    }
}

```

Default behavior of gitleaks in jenkins
By default, gitleaks exist with a non-zero code from a “sh” step causes the pipeline to fail and stop.

While ``|| true ``lets the pipeline continue, you should still review the report to address real secrets. Ignoring leaks can lead to security incidents.
