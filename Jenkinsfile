pipeline {
    environment {
    registry = 'harbor-core.az.tillman.wtf/tillmanwtf/homebridge'
    registryCredential = 'harbor-core.az.tillman.wtf'
    dockerImage = ''
    }
    agent {
        kubernetes {
            yamlFile 'KubernetesBuildPod.yaml'
        }
    }
    stages {
        stage('Git Clone') {
            steps {
                checkout scmGit(branches: [[name: '*/latest']], userRemoteConfigs: [[credentialsId: 'github-briantillman', url: 'git@github.com:BrianTillman/docker-homebridge.git']])
            }
        }
        stage('Build Container Image') {
            steps {
                container('docker') {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }
        }
        stage('Publish Container Image') {
            steps {
                container('docker') {
                    script {
                        docker.withRegistry( "https://" + registry, registryCredential) {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}