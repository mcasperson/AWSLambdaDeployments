pipeline {
    agent any
    stages {
        stage ('Build') {
            if (isUnix()) {
                steps {
                    sh "Octo pack --format zip --id AwsLambdaHello --version 0.0.${env.BUILD_NUMBER}"
                }
            } else {
                steps {
                    powershell "Octo pack --format zip --id AwsLambdaHello --version 0.0.${env.BUILD_NUMBER}"
                }
            }
        }

        stage ('Deploy to Octopus') {
            withCredentials([
                string(credentialsId: 'APIKey', variable: 'APIKey'),
                string(credentialsId: 'OctopusServer', variable: 'OctopusServer')
            ]) {
                if (isUnix()) {
                    steps {
                        sh """
                            Octo push --package AwsLambdaHello.0.0.${env.BUILD_NUMBER}.zip --replace-existing --server ${OctopusServer} --apiKey ${APIKey}
                            Octo create-project --name "Aws Lambda Hello" --lifecycle "Default Lifecycle" --projectGroup "Default Project Group" --ignoreIfExists --server ${OctopusServer} --apiKey ${APIKey}
                            Octo create-release --project "Aws Lambda Hello" --server ${OctopusServer} --apiKey ${APIKey}
                            Octo deploy-release --project "Aws Lambda Hello" --version latest --deployto Dev --server ${OctopusServer} --apiKey ${APIKey}
                        """
                    }
                } else {
                    steps {
                        powershell """
                            Octo push --package AwsLambdaHello.0.0.${env.BUILD_NUMBER}.zip --replace-existing --server ${OctopusServer} --apiKey ${APIKey}
                            Octo create-project --name "Aws Lambda Hello" --lifecycle "Default Lifecycle" --projectGroup "Default Project Group" --ignoreIfExists --server ${OctopusServer} --apiKey ${APIKey}
                            Octo create-release --project "Aws Lambda Hello" --server ${OctopusServer} --apiKey ${APIKey}
                            Octo deploy-release --project "Aws Lambda Hello" --version latest --deployto Dev --server ${OctopusServer} --apiKey ${APIKey}
                        """
                    }
                }
            }
        }
    }
}