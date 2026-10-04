pipeline {
    agent {
         node { label 'jenkins-test' } 
    } 
     environment { 
        NODE = 'jenkins-test'
    }
    options {
         disableConcurrentBuilds() 
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
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh """ 
                        echo 'Deploying....'
                        echo 'Environment: ${NODE}'
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