pipeline {
    agent { label 'arm64' }

    environment {
        ROS_SETUP = '/opt/ros/humble/setup.bash'
        ROS2_WS_SETUP = '/opt/ros2_ws/install/setup.bash'
        ALERT_EMAIL = credentials('7f303f7b-bb98-44b0-83b9-333ffe60c73f')
    }

    stages {

        stage('Check installed packages') {
            steps {
                script {
                    // Check the installed packages
                    sh 'dpkg -l'
                }
            }
        }
        
        stage('SonarQube') {
            steps {
                script {
                    // Run the SonarQube analysis
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run the build script
                    sh './build.sh'
                }
            }
        }
    }

    post {
        always {
            // Archive the build artifacts
            archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
            mail to: "${ALERT_EMAIL}",
                 subject: "Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The build has failed. Please check the Jenkins console output for more details."
        }
    }
}