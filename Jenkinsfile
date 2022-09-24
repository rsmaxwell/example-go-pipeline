pipeline {
  agent {
    kubernetes {
      yamlFile 'KubernetesPod.yaml'
    }
  }

  environment {
    PROJECT_NAME = "example-go"
    PROJECT_DIR = "${WORKSPACE}/project/src/github.com/rsmaxwell/${PROJECT_NAME}"
  }

  stages {

    stage('prepare') {
      steps {
        container('tools') {
          dir('project/src/github.com/rsmaxwell/example-go') {
            echo 'preparing the application'
            checkout([
              $class: 'GitSCM', 
              branches: [[name: '*/main']], 
              extensions: [], 
              userRemoteConfigs: [[url: 'https://github.com/rsmaxwell/example-go']]
            ])
            sh('echo "pwd = $(pwd)"')

            sh('echo "ls -al"')
            sh('ls -al')

            sh('echo "ls -al ./scripts"')
            sh('ls -al ./scripts')

            sh('echo "PROJECT_NAME = $PROJECT_NAME"')
            sh('echo "PROJECT_DIR  = $PROJECT_DIR"')
            sh('./scripts/prepare.sh')
          }
        }
      }
    }

    stage('build') {
      steps {
        container('golang') {
          dir('project/src/github.com/rsmaxwell/example-go') {
            echo 'building the application'
            
            sh('echo "ls -al /"')
            sh('ls -al /')
            
            sh('echo "ls -al /go"')
            sh('ls -al /go')
            
            sh('echo "ls -al /go/bin"')
            sh('ls -al /go/bin')
            
            sh('echo "ls -al /go-xxx"')
            sh('ls -al /go-xxx')

            sh('echo "PROJECT_NAME = $PROJECT_NAME"')
            sh('echo "PROJECT_DIR  = $PROJECT_DIR"')
            sh('echo "GOPATH = $GOPATH"')
            sh('echo "GOROOT = $GOROOT"')


            sh('go version"')
            sh('go env"')

            sh('./scripts/build.sh')
          }
        }
      }
    }

    stage('test') {
      steps {
        container('tools') {
          dir('project/src/github.com/rsmaxwell/example-go') {
            echo 'testing the application'
            sh('./scripts/test.sh')
          }
        }
      }
    }

    stage('package') {
      steps {
        container('tools') {
          dir('project/src/github.com/rsmaxwell/example-go') {
            echo 'packaging the application'
            sh('./scripts/package.sh')
          }
        }
      }
    }

    stage('deploy') {
      steps {
        container('maven') {
          dir('project/src/github.com/rsmaxwell/example-go') {
            echo 'deploying the application'
            sh('./scripts/deploy.sh')
          }
        }
      }
    }
  }
}
