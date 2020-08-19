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
                            parameters: [choice(name: 'codeanalysis', choices: ['Yes','No'].join('\n'), description: 'Do you wan to perform code quality analysis?')]
                    echo "${env.codeanalysis}"
                    if (env.codeanalysis=='Yes'){
                             echo "Build Project"
                             withSonarQubeEnv('sonar') {
                                       powershell label: '', script: 'mvn package sonar:sonar'
                                     }                            
                             }
                        else{
                            powershell label: '', script: 'mvn clean package'
                        }
                }
            }
            }
            }
            post {
                    always {
                        archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                        junit '**/*.xml'
                        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'C:/Program Files (x86)/Jenkins/workspace/HappyT-pipeline/target/site/jacoco', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
                    }
                }
            
         }
