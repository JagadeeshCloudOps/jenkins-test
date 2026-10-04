pipeline {

    agent {
         node { label 'nodejs-jenkins-test' } 
    } 

     environment { 
        appVersion = ""
        registryUrl = ""
        ACC_ID = "567579393458"
        AWS_REGION = "us-east-1"
        ECR_REPO_NAME = "nodejs/jenkins-test"
    } 

    options {
         disableConcurrentBuilds() 
          timeout(time: 5, unit: 'MINUTES')
    }

/*     parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    } */

    stages {
        stage ('read version') {
            steps {
                script {
                    // Read the JSON file from the workspace root
                    def packageJson = readJSON file: 'package.json'
                    
                    // Access the version property
                    appVersion = packageJson.version
                    
                    // Print it or assign it to an environment variable
                    echo "The current version is: ${appVersion}"
                    
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                        sh """ 
                            npm install
                        """
                }
            }
        }
        /* stage('Build Image') {
            steps {
                script {
                    sh """ 
                        docker build -t ${ECR_REPO_NAME}:${appVersion} .
                        
                    """
                }
            }
        } */
        stage('Build and Push to Amazon ECR') {
            steps {
                // 3. Authenticate and push using the Pipeline: AWS Steps plugin
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    script {
                        def registryUrl = "${ACC_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                        
                        // Login to ECR
                        sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${registryUrl}"
                        
                        sh "docker build -t ${registryUrl}/${ECR_REPO_NAME}:${appVersion} ."
                        
                        // Tag image for the remote repository
                        sh "docker push ${registryUrl}/${ECR_REPO_NAME}:${appVersion}"
                    
                    }
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return params.Deploy == true }
            }
            steps {
                script {
                  sh """ 
                      echo 'Deploying....'
                      
                   """
                }
            }
        }
    }

     post { 
        always { 
            echo 'I will always say Hello again!'
        }
        success {
            echo 'I will say Hello only if pipeline successful!'
        }
         failure {
            echo 'I will say Hello only if pipeline fails!'
        }
    }

}