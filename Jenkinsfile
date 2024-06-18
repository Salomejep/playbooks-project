pipeline{
    agent any

    stages{
        stage('zip the file'){
            steps{
                sh 'zip ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
                sh 'ls -l'
            }
        }
        stage('upload artifacts to jfrog'){
            steps{
                sh 'curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd -T \
                 ansible-${BUILD_ID}.zip \
                  "http://3.239.159.241:8081/artifactory/ansible/ansible-${BUILD_ID}.zip"'
            }
        }
        stage('publish ansible syntax'){
            steps{
                sh 'sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', \
                transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: \
                'ls', execTimeout: 120000, flatten: false, makeEmptyDirs: false, \
                noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: \
                '', remoteDirectorySDF: false, removePrefix: '.', \
                sourceFiles: 'ansible-${BUILD_ID}.zip')], usePromotionTimestamp: false, \
                useWorkspaceInPromotion: false, verbose: false)])'
            }
        }
    }
}