pipeline{
    agent any
    stages{
            stage('Build'){
                tools{
                    jdk 'jdk1.8'
                }
                steps
                {
                    script {
                    env.codeanalysis = input message: 'User input required', ok: 'OK!',
                            parameters: [choice(name: 'codeanalysis', choices: ['Yes','No'].join('\n'), description: 'Do you want to perform code quality analysis?')]
                    echo "${env.codeanalysis}"
                    if (env.codeanalysis=='Yes'){
                             echo "Performing code quality analysis and building the project"
                             withSonarQubeEnv('sonar') {
                                       powershell label: '', script: 'mvn package sonar:sonar'
                                     }                            
                             }
                        else{
                            echo "Not performing code quality analysis, building the project"
                            powershell label: '', script: 'mvn clean package'
                        }
                }
            }
            }
            }
            post {
                    always {
                        echo "Archiving artifacts"
                        archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                        script {
                        env.deploy = input message: 'User input required', ok: 'OK!',
                            parameters: [choice(name: 'deploy', choices: ['Yes','No'].join('\n'), description: 'Do you want to deploy on Tomcat?')]
                        echo "${env.codeanalysis}"
                        if (env.deploy=='Yes'){
                             echo "Deploying on Tomcat"
                             deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8081/')], contextPath: 'HappyT', onFailure: false, war: '**/*.war'
                             }
                            else{
                                echo "Not Deploying on TomCat"
                            }
   
                         }
                        emailext body: 'Pipeline successful', subject: 'Pipeline successful', to: 'mahimamalik98@gmail.com'                   
                      }
                    failure {
                        emailext body: 'Pipeline unsuccessful', subject: 'Pipeline unsuccessful', to: 'mahimamalik98@gmail.com'
                    }
                }
 }
