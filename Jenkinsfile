pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Update system and check Docker Compose version
                    sh 'apt-get update'
                    sh 'apt-get upgrade -y'
                    sh 'docker compose version'
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    // Start services and expose them to localhost
                    sh 'docker compose up -d'
                    sh 'docker ps'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Install Python and dependencies
                    sh 'apt-get install -y python3 python3-venv python3-pip'

                    sh '''
                        python3 -m venv .venv
                        . .venv/bin/activate
                        pip install pytest selenium

                        # Run the test script
                        python test_aimanTest.py
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                // Keep services running so results are visible on localhost
                sh '''
                    echo "Services are running. Access results at:"
                    echo "Backend: http://localhost:8000"
                    echo "Frontend: http://localhost:81"
                '''
            }
        }
    }
}
