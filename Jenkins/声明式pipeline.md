```groovy
#! groopvy
import java.time.LocalDateTime
import java.time.format.DateTimeFormatter

/**
 * 默认tag
 * @return
 **/
@NonCPS
def getDefaultTag(){
    LocalDateTime localDateTime = LocalDateTime.now()
    localDateTime.format(DateTimeFormatter.ofPattern('yyyyMMddmmss',Locale.CHINESE))
}

/**
 * 声明式流水线脚本
 * @return
 **/
def call(body){
    def config = [:]
    body.resolveStrategy = Closure.DELEGATE_FIRST
    body.delegate = config
    body()

    assert  config.name: 'name不能为空'

    def needHost = config.needHost ?:'false'
    def imageName = params.IMAGE_NAME ?:config.name
    def deployENV = params.DEPLOY_ENV ?:'dev'
    def type = config.type ?:'maven'
    def coverage = config.coverage?:'10'

    def imageTag = params.IMAGE_TAG ?:getDefaultTag()
    def baseImage = params.BASE_IMAGE ?:env.BASE_IMAGE
    def cmd = params.CMD ?:''
    assert  baseImage:'基础镜像不能为空'

    pipeline{
        agent{
            kubernetes{
                label 'mypod'
                cloud 'kubernetes'
                yaml '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: jnlp
    image: jenkins/jnlp-slave:alpine
    command:
    - cat
    tty: true
'''
            }
        }
        stages{
            stage('代码检出'){
                cotainer('jnlp'){
                    checkout scm
                }
            }
        }
    }
}
```

这段声明式pipeline存在问题：会不停去创建pod，而且pod都是active的，就是不跑后面的stage,去掉jnlp解决。

