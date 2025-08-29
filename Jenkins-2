pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                dir('/mnt/workspace/2025Q1') {
                    echo "Cleaning workspace..."
                    sh 'rm -rf /mnt/workspace/2025Q1/*'

                    echo "Cloning repository..."
                    git(
                        branch: '2025Q1',
                        credentialsId: 'e4259c9b-8a98-4c29-8324-6ac34de2c01a',
                        url: 'https://github.com/KunalGKNY/Docker-Multibranch.git'
                    )
                }
            }
        }

 

        stage('Docker container creations') {
            steps {
                sh '''
                    # Create container C1 if not exists
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
                '''
            }
        }

        stage('Remove index.html from containers') {
            steps {
                sh '''
                    docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
                '''
            }
        }

        stage('Copying the file into the container') {
            steps {
                sh '''
                    docker cp /mnt/workspace/2025Q1/index.html C1:/usr/local/apache2/htdocs
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                '''
            }
        }
    }
}
