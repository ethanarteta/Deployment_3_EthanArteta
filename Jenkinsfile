pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    // Set up and activate a virtual environment
                    sh '''
                        python3 -m venv test3
                        source test3/bin/activate
                        pip install pip --upgrade
                        pip install -r requirements.txt
                    '''
                }
                script {
                    // Export environment variable and start Flask app in the background
                    sh 'export FLASK_APP=application'
                    sh 'flask run &'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Activate the virtual environment and run pytest
                    sh 'source test3/bin/activate'
                    sh 'py.test --verbose --junit-xml test-reports/results.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Deploy using Elastic Beanstalk (EB)
                    sh '/var/lib/jenkins/.local/bin/eb deploy'
                }
            }
        }
    }
    post {
        always {
            junit 'test-reports/results.xml'
        }
    }
}

