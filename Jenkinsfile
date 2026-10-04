pipeline {

    agent {
         node { label 'jenkins-test' } 
    } 

    environment { 
        NODE = 'jenkins-test'
    }

    options {
         disableConcurrentBuilds() 
          timeout(time: 5, unit: 'MINUTES')
    }

    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }

    stages {
        stage('Build') {
            steps {
                script {
                        sh """ 
                            echo 'Building..'
                            echo 'Environment: ${NODE}'
                        """
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh """ 
                        echo 'Testing..'
                        echo 'Environment: ${NODE}'
                        echo "Hello ${params.PERSON}"
                        echo "Biography: ${params.BIOGRAPHY}"
                        echo "Deploy: ${params.Deploy}"
                        echo "Choice: ${params.CHOICE}"
                        echo "Password: ${params.PASSWORD}"
                    """
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return params.Deploy == true }
            }
            // steps {
            //     script {
            //         sh """ 
            //             echo 'Deploying....'
            //             echo 'Environment: ${NODE}'
            //         """
            //     }
            // }
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