node {
    def customimage

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
        sh 'mkdir -p Oboo-Source'
                
        /* Checkout Oboo-Source since that's actually going to do the building...*/
        
        dir("Oboo-Source"){
            git 'https://github.com/ouellettetech/Oboo-Source.git'
            sh "pwd"
            sh "ls"
            sh "whoami"
            sh "env"
            sh "cp -r /lib/sshKeys ."
        }
    }

    stage('Build image') {
        /* This builds the build image; synonymous to
         * docker build on the command line */

        dir("Oboo-Source"){
            customimage = docker.build("oboo-package:${env.BUILD_ID}")
        }
        
        sh "mkdir -p output"
        customimage.inside {
            echo "Before Build...."
            sh "pwd"
            sh "ls"
            sh '/root/source/build.sh'
            echo "After Build...."
        }
        sh "echo Outside build..."
    }

    stage('Save Artifacts') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. 
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
        */
        success {
                    //junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'output/**'
                }
    }
    
    
/*    
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
  */
}
