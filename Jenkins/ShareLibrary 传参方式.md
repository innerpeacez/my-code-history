### 目前有两种传参方式：

- MAP 传递
- DSL-step 传递

####MAP 传递  

**Jenkinsfile**

```
myDeliveryPipeline(branch: 'master', scmUrl: 'ssh://git@myScmServer.com/repos/myRepo.git',
                   email: 'team@example.com', serverPort: '8080',
                   developmentServer: 'dev-myproject.mycompany.com',
                   stagingServer: 'staging-myproject.mycompany.com',
                   productionServer: 'production-myproject.mycompany.com')
```

**vars/myDeliveryPipeline.groovy**

```
def call(Map pipelineParams) {

    pipeline {
        agent any
        stages {
            stage('checkout git') {
                steps {
                    git branch: pipelineParams.branch, credentialsId: 'GitCredentials', url: pipelineParams.scmUrl
                }
            }

            stage('build') {
                steps {
                    sh 'mvn clean package -DskipTests=true'
                }
            }

            stage ('test') {
                steps {
                    parallel (
                        "unit tests": { sh 'mvn test' },
                        "integration tests": { sh 'mvn integration-test' }
                    )
                }
            }

            stage('deploy developmentServer'){
                steps {
                    deploy(pipelineParams.developmentServer, pipelineParams.serverPort)
                }
            }

            stage('deploy staging'){
                steps {
                    deploy(pipelineParams.stagingServer, pipelineParams.serverPort)
                }
            }

            stage('deploy production'){
                steps {
                    deploy(pipelineParams.productionServer, pipelineParams.serverPort)
                }
            }
        }
        post {
            failure {
                mail to: pipelineParams.email, subject: 'Pipeline failed', body: "${env.BUILD_URL}"
            }
        }
    }
}
```

####DSL-step 传递

**Jenkinsfile**

```
myDeliveryPipeline {
    branch = 'master'
    scmUrl = 'ssh://git@myScmServer.com/repos/myRepo.git'
    email = 'team@example.com'
    serverPort = '8080'
    developmentServer = 'dev-myproject.mycompany.com'
    stagingServer = 'staging-myproject.mycompany.com'
    productionServer = 'production-myproject.mycompany.com'
}
```

**vars/myDeliveryPipeline.groovy**

```
def call(body) {
    // evaluate the body block, and collect configuration into the object
    def pipelineParams= [:]
    body.resolveStrategy = Closure.DELEGATE_FIRST
    body.delegate = pipelineParams
    body()

    pipeline {
        // our complete declarative pipeline can go in here
        ...
    }
}
```

