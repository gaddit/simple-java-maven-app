pipeline {
    agent {
        label "master"
    }

    environment {
      NEXUS_VERSION = "nexus3"
      NEXUS_PROTOCOL = "http"
      // Where your Nexus is running. 'nexus-3' is defined in the docker-compose file
      NEXUS_URL = "nexus.gaddit.se:80"
      // Repository where we will upload the artifact
      NEXUS_REPOSITORY = "simple-java-maven-app"
      // Jenkins credential id to authenticate to Nexus OSS
      NEXUS_CREDENTIAL_ID = "testuser"
      NEXUS_SCRIPT = "maven-create-hosted"
    }
    stages {
      stage("mvn build") {
          steps {
              script {
                  // If you are using Windows then you should use "bat" step
                  // Since unit testing is out of the scope we skip them
                  sh "mvn package -DskipTests=true"
              }
          }
      }

      stage("publish to nexus") {
        steps {
          script {
              // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
              pom = readMavenPom file: "pom.xml";
              // Find built artifact under target folder
              filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
              // Print some info from the artifact found
              echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
              // Extract the path from the File found
              artifactPath = filesByGlob[0].path;
              // Assign to a boolean response verifying If the artifact name exists
              artifactExists = fileExists artifactPath;

              if(artifactExists) {
                  echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                  
              }

            } else {
              error "*** File: ${artifactPath}, could not be found";
          }
        }
      }
    }
  }
}
