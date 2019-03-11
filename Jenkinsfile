pipeline {
   agent any
   stages {
    stage('Build') {
        steps {
            script {
                currentBuild.displayName = "#"+env.BUILD_NUMBER+"["+params.env+"]"
                echo "This is a new change from template"
            }
            // Run the maven build
            // sh label: 'chmod', script: 'chmod 744 ./myscript.sh'
        }
    }
    stage('Run') {
        steps {
            withCredentials([usernamePassword(credentialsId: '9ef7f06b-f7a6-4a2f-ad22-e00aff750b9d', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                echo 'uname=$USERNAME pwd=$PASSWORD'
                sh label: 'run script', script: 'bash ./myscript.1.sh params.env $USERNAME:$PASSWORD 2>&1 | tee -a output.txt'
            }
        }
    }
    stage('Commit') {
        steps {
            script {
                tagname = params.env + "_" + env.BUILD_NUMBER
            }
            sh script: "git config --global user.email 'rodvina@gmail.com'"
            sh script: "git config --global user.name 'Rodney Odvina'"

            //sh script: "tagname=$params.env_$env.BUILD_NUMBER"
            sh script: "git add ."
            sh script: "git commit -m 'Jenkins output'"
            sh script: "git tag -a $tagname -m 'Jenkins output'"
            sh script: "git push"
            

            // withCredentials([usernamePassword(credentialsId: '9ef7f06b-f7a6-4a2f-ad22-e00aff750b9d', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
            //     sh script: "git remote set-url --push origin https://$USERNAME:$PASSWORD@github.com/rodvina/jenkins-101.git"
            //     sh script: "git push origin HEAD:master --tags"
            // }
            
        }
    }
   }
   
}