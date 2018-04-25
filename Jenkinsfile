node {

    withMaven(maven:'maven') {

        stage('Checkout') {
            git url: 'https://github.com/piomin/sample-spring-microservices.git', credentialsId: 'github-piomin', branch: 'master'
        }

        stage('Build') {
            sh 'mvn clean install'

            def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }

        stage('Image') {
            dir ('gateway-service') {
                def app = docker.build "naveengoswami/gateway-service:${env.version}"
            }
        }

        stage ('Run') {
            docker.image("naveengoswami/gateway-service:${env.version}").run('-p 8765:8765 -h gateway --name gateway --link discovery --link account --link customer')
        }
     

    }

}