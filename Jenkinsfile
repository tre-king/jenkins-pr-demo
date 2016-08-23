node {

    currentBuild.result = 'SUCCESS'
    
    try {

	stage 'Print Environment'
            sh 'env'

        stage 'checkout'
            checkout([$class: 'GitSCM', branches: [[name: 'feature/Jenkins-Demo-PR']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '6f7b9d02-7287-4d74-bbad-133721eaf1f0', url: 'https://ha-king@bitbucket.org/ha-king/jenkins-pr-demo.git']]])

        stage 'validate cfn template'
            sh 'aws cloudformation validate-template --template-body file://Jenkins-Demo-PR.json'

        stage 'deploy ephemeral stack'
            sh 'author=$(git --no-pager show -s --format="%an"); aws cloudformation create-stack --stack-name Temp-$(date +%s)-Jenkins-PoC --tags Key=AuthorName,Value="$author" --template-body file://Jenkins-Demo-PR.json'

    } catch(e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}
