pipeline {

    agent {
         node { label 'nodejs-jenkins-test' } 
    } 

     environment { 
        appVersion = ""
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
        stage('Build Image') {
            steps {
                script {
                    sh """ 
                        docker build -t my-nodejs-app:${appVersion} .
                        
                    """
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