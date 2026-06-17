pipeline {
agent any

```
parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
    choice(name: 'DEPLOY_ENV', choices: ['staging', 'production'], description: 'Deployment environment')
}

stages {
    stage('Checkout') {
        steps {
            git branch: params.BRANCH_NAME, url: 'git@github.com:apavan05/cloudbox-app.git'
        }
    }
}
```

}
