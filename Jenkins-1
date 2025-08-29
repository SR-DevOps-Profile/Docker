pipeline {
    agent none

    stages {
        stage('Checkout Code') {
            agent { label 'built-in' }
            steps {
                dir('/mnt/workspace1/') {
                    echo "Cleaning workspace..."
                    sh 'rm -rf /mnt/workspace1/*'

                    echo "Cloning repository..."
                    git(
                        branch: 'main',
                        credentialsId: '441c171e-1195-4d3e-ac27-c9e1f15d1728',
                        url: 'https://github.com/KunalGKNY/Docker-Assignment-2.git'
                    )
                    stash name: 'html-files', includes: 'index.html'
                }
            }
        }

        stage('Docker container creation on slave1') {
            agent { label 'slave1' }
            steps {
                cleanWs()
                unstash 'html-files'
                sh '''
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
                    docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
                    docker cp index.html C1:/usr/local/apache2/htdocs
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                '''
            }
        }

        stage('Docker container creation on slave2') {
            agent { label 'slave2' }
            steps {
                cleanWs()
                unstash 'html-files'
                sh '''
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 90:80 --name C1 httpd
                    docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
                    docker cp index.html C1:/usr/local/apache2/htdocs
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                '''
            }
        }
    }
}



// usermod -aG docker ec2-user
 //   groups ec2-user
 //    pkill -f remoting.jar
