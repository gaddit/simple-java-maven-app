pipeline {
    agent {
        label "Built-In Node"
    }
    environment {
      NEXUS_VERSION = "nexus3"
      NEXUS_PROTOCOL = "http"
      // Where your Nexus is running. 'nexus-3' is defined in the docker-compose file
      NEXUS_URL = "nexus.gaddit.se:80"
      // Repository where we will upload the artifact
      NEXUS_REPOSITORY = "simple-java-maven-app"
      // Jenkins credential id to authenticate to Nexus OSS
      NEXUS_CREDENTIAL_ID = "anonymous"
      NEXUS_SCRIPT = "maven-create-hosted"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
