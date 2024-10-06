pipeline{
    agent any

    stages{
        stage('Build'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    echo "Test stage"
                    test -f build/index.html

                    if test -f build/index.html; then
                        echo "File exists."
                    else                        
                        exit 2
                    fi

                    echo "Before running test"
                    npm test
                    echo "After running test"
                '''
            }
        }

        post{
            always{
                junit 'test-results/junit.xml'
            }
        }
    }
}