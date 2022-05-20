pipeline {
  agent {
    kubernetes {
      yamlFile 'KubernetesPod.yaml'
    }
  }

  environment {
    PROJECT_NAME = "example-go"
    GOPATH = "${WORKSPACE}"
    PATH = "${PATH}:${GOPATH}/bin"
    PROJECT_DIR = "${WORKSPACE}/src/github.com/rsmaxwell/${PROJECT_NAME}"
  }

  stages {

    stage('prepare') {
      steps {
        container('tools') {
          dir('${env.PROJECT_DIR}') {
            echo 'preparing the application'
            checkout([
              $class: 'GitSCM', 
              branches: [[name: '*/main']], 
              extensions: [], 
              userRemoteConfigs: [[url: 'https://github.com/rsmaxwell/example-go']]
            ])
            sh('./scripts/prepare.sh')
          }
        }
      }
    }

    stage('build') {
      steps {
        container('go') {
          dir('${env.PROJECT_DIR}') {
            echo 'building the application'
            sh('./scripts/build.sh')
          }
        }
      }
    }

    stage('test') {
      steps {
        container('tools') {
          dir('${env.PROJECT_DIR}') {
            echo 'testing the application'
            sh('./scripts/test.sh')
          }
        }
      }
    }

    stage('package') {
      steps {
        container('tools') {
          dir('${env.PROJECT_DIR}') {
            echo 'packaging the application'
            sh('./scripts/package.sh')
          }
        }
      }
    }

    stage('deploy') {
      steps {
        container('maven') {
          dir('${env.PROJECT_DIR}') {
            echo 'deploying the application'
            sh('./scripts/deploy.sh')
          }
        }
      }
    }
  }
}
