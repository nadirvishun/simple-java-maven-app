pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('deploy'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: '192.168.1.32-ssh', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cp /home/test/my-app-1.0-SNAPSHOT.jar /home/test/my-app.jar', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/home/test', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/my-app-1.0-SNAPSHOT.jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}