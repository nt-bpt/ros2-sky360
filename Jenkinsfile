pipeline {
    agent { label 'arm64' }

    environment {
        ROS_SETUP = '/opt/ros/humble/setup.bash'
        ROS2_WS_SETUP = '/opt/ros2_ws/install/setup.bash'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    // Source the ROS setup files
                    sh "source ${ROS_SETUP}"
                    sh "source ${ROS2_WS_SETUP}"
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
            mail to: 'jenkins@bluffpointtech.com',
                 subject: "Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The build has failed. Please check the Jenkins console output for more details."
        }
    }
}