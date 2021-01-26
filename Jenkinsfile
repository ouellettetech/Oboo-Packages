pipeline {
    agent any

    //tools {
        // Install the Maven version configured as "M3" and add it to the path.
        //maven "M3"
    //}

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                sh "mkdir -p Oboo-Source"
                
                dir("Oboo-Source"){
                  dir "Oboo-Source"
                  git 'https://github.com/ouellettetech/Oboo-Source.git'
                  sh "pwd"
                  // Run Maven on a Unix agent.
                  sh "ls"
                  sh "whoami"
                  sh "env"
                  sh "cp -r /lib/sshKeys ."
                  
                  def customImage = docker.build("my-image:${env.BUILD_ID}")
                }
                
                sh "mkdir -p output"
                
                customImage.inside('-e "PACKAGE_CHECKOUT=$GIT_COMMIT" -v `pwd`/output:/root/source/bin $BUILD_TAG') {
                    sh build.sh
                }

                sh "du output"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    //junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'output/**'
                }
            }
        }
    }
}
