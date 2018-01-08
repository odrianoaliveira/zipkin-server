timestamps {
    node('docker-ms') {
    def extcode
        try {
            stage ('Checkout') {
                sh 'sudo rm -rf $WORKSPACE/.git*'
                sh 'sudo rm -rf $WORKSPACE/*'

                //BACALHAU
                sh 'sudo chmod 777 -f -R .git/ | echo "Não tem .git"'
                checkout scm
                sh 'sudo chmod 777 -f -R .git/ | echo "Não tem .git"'

                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'devops']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'DADHX01_SSH', url: 'git@dadhx01:arquitetura-digital/jenkins.git']]])
                extcode = load 'devops/deployMS.groovy'
            }
            extcode.getJenkinsFileWithoutDeploy()
        } catch (e) {
            currentBuild.result = "FAILED"
            throw e
        } finally {
            extcode.notifyBuild(currentBuild.result)
        }
    }
}
