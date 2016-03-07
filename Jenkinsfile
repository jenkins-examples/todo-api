node('jdk8')
{
   env.PATH="${tool 'mvn 3.3.3.x'}/bin:${env.PATH}"
   stage 'development'
   sh 'mvn clean install'
}

node('jdk8')
{
    stage 'quality-analysis'

	checkpoint('Sonar  - Complete')

    stage 'quality-assurance'

	parallel(integrationTests: {
        runWithServer {url ->
            sh "mvn -f sometests/pom.xml test -Durl=${url} -Dduration=30"
        }
    }, functionalTests: {
        runWithServer {url ->
            sh "mvn -f sometests/pom.xml test -Durl=${url} -Dduration=20"
        }
    })

    checkpoint('Integration and Functional Tests - Complete')
}


input message: "QA Team Approval?"

stage 'performance'

checkpoint('Performance Tests - Complete')

stage 'production'