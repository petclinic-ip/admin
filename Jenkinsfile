node("maven-label") {
    def mvnHome
    stage('Preparation') { // for display purposes
      
        git 'https://github.com/petclinic-ip/admin.git'
        
        mvnHome = tool 'maven-3.6.3'
    }
    stage('Build') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dsonar.host.url=http://ip-172-31-11-7.ap-south-1.compute.internal:9000/ -Dmaven.test.failure.ignore clean deploy sonar:sonar'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
    }
}
