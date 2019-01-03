pipeline {
  agent any

  options {
    timeout(time: 10, unit: 'MINUTES')
    ansiColor('xterm')
    checkoutToSubdirectory('traefik-forward-auth')
  }

  stages {
    stage('Checkout halkeye/helm-charts') {
      environment {
        GITHUB = credentials('github-halkeye')
      }
      steps {
        dir('helm-charts') {
          sh 'git clone -b gh-pages https://${GITHUB_USR}:${GITHUB_PSW}@github.com/halkeye/helm-charts.git .'
        }
      }
    }

    stage('Build') {
      steps {
        dir('traefik-forward-auth') {
          script {
            docker.image('dtzar/helm-kubectl').inside {
              sh 'helm init -c'
              sh 'helm lint .'
              sh 'helm package .'
            }
          }
          stash(name:'helm-packages', allowEmpty: false, includes: '*.tgz')
        }
        dir('helm-charts') {
          dir('traefik-forward-auth') {
            unstash(name:'helm-packages')
          }
          script {
            docker.image('dtzar/helm-kubectl').inside {
              sh 'helm repo index ./'
            }
          }
        }
      }
    }

    stage('Commit') {
      steps {
        dir('helm-charts') {
          sh 'git config --global user.email "jenkins@gavinmogan.com"'
          sh 'git config --global user.name "Jenkins"'
          sh 'git add index.yaml traefik-forward-auth'
          sh 'git commit -m "Adding package"'
        }
      }
    }

    stage('Deploy') {
      when {
        branch 'master'
      }
      environment {
        GITHUB = credentials('github-halkeye')
      }
      steps {
        dir('helm-charts') {
          sh 'git config --global user.email "jenkins@gavinmogan.com"'
          sh 'git config --global user.name "Jenkins"'
          sh 'git config --global push.default simple'
          sh 'git push origin gh-pages'
        }
      }
    }
  }
}
