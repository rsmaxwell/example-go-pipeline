pipeline {
  agent {
    kubernetes {
      yamlFile 'KubernetesPod.yaml'
    }
  }

  environment {
    PROJECT_NAME = "example-go"
    GOPATH = "${WORKSPACE}/project"
    PATH = "${PATH}:${GOPATH}/bin"
    PROJECT_DIR = "${GOPATH}/src/github.com/rsmaxwell/${PROJECT_NAME}"
  }

  stages {

    stage('prepare') {
      steps {
        container('tools') {
          dir('project') {
            echo 'preparing the application'
            checkout([
              $class: 'GitSCM', 
              branches: [[name: '*/main']], 
              extensions: [], 
              userRemoteConfigs: [[url: 'https://github.com/rsmaxwell/example-go']]
            ])
            sh('pwd')
            sh('ls -al')
            sh('ls -al ./scripts')
            sh('echo $WORKSPACE')
            sh('echo $PROJECT_DIR')
            sh('tree')
            sh('./scripts/prepare.sh')
          }
        }
      }
    }

    stage('build') {
      steps {
        container('golang') {
          dir('project') {
            echo 'building the application'
            sh('./scripts/build.sh')
          }
        }
      }
    }

    stage('test') {
      steps {
        container('tools') {
          dir('project') {
            echo 'testing the application'
            sh('./scripts/test.sh')
          }
        }
      }
    }

    stage('package') {
      steps {
        container('tools') {
          dir('project') {
            echo 'packaging the application'
            sh('./scripts/package.sh')
          }
        }
      }
    }

    stage('deploy') {
      steps {
        container('maven') {
          dir('project') {
            echo 'deploying the application'
            sh('./scripts/deploy.sh')
          }
        }
      }
    }
  }
}
