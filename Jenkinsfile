node {
environment {
     registry = "sagdeshm1/ms"
     registryCredential = ‘docker-hub-credentials’
 }

    withMaven(maven:'mvn') {

        stage('Checkout') {
            git url: 'https://github.com/msmb4u/EquityTradePortfolio.git', credentialsId: 'github-credentials', branch: 'master'
        }

        stage('Build') {
            sh 'mvn clean install'

            def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }

stage('Deploy Image') {
  steps{
    script {
      docker.withRegistry( '', registryCredential ) {
        dockerImage.push()
      }
    }
  }
}
stage('Remove Unused docker image') {
  steps{
    sh "docker rmi $registry:$BUILD_NUMBER"
  }
}
    }

}