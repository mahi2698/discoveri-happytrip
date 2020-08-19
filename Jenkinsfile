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
                }
            }
            }
            stage('Archive'){
                steps
                {
                    echo "Archive Artifact"
                    archiveArtifacts artifacts: 'C:/Program Files (x86)/Jenkins/workspace/HappyT-pipeline/target/*.jar', followSymlinks: false
                }
            }
            stage('Publish JUnit'){
                steps
                {
                    echo "Publish JUnit"
                    junit 'C:/Program Files (x86)/Jenkins/workspace/HappyT-pipeline/target/surefire-reports/*.xml'
                }
            }
            stage('Publish HTML'){
                steps
                {
                    echo "Publish HTML"
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'C:/Program Files (x86)/Jenkins/workspace/HappyT-pipeline/target/site/jacoco', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
                }
            }
         }
}
