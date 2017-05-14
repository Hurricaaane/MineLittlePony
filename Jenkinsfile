pipeline {
    agent {
        docker 'dodb/jenkins-java-gradle-slave'
    }
    stages {
        stage('build') {
            steps {
                updateCommitStatus('PENDING')
                sh './gradlew build'
            }
        }
    }
    post {
        success {
            archive 'build/libs/**.litemod'
            updateCommitStatus('SUCCESS')
        }
        failure {
            updateCommitStatus('FAILURE')
        }
        unstable {
            updateCommitStatus('FAILURE')
        }
    }
}

def updateCommitStatus(status) {
    step([
        $class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: 'jenkins'],
        statusResultSource: [$class: 'ConditionalStatusResultSource', results: [[$class: 'AnyBuildResult', message: 'jenkins-pipeline', state: status]]]
    ])
}