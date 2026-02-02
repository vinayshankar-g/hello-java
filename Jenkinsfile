pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out from Git'
            }
        }

        stage('Compile Java') {
            steps {
                bat '''
                wsl rm -rf build
                wsl mkdir -p build
                wsl javac -d build src/Hello.java
                wsl jar cfe hello.jar Hello -C build .
                '''
            }
        }

        stage('Prepare Package Directory') {
            steps {
                bat '''
                wsl mkdir -p package/usr/local/bin
                wsl cp hello.jar package/usr/local/bin/
                '''
            }
        }

        stage('Build DEB using FPM') {
            steps {
                bat '''
                wsl fpm -s dir -t deb -n hello-java -v 1.0.%BUILD_NUMBER% --prefix=/ -C package
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '*.deb'
        }
    }
}
