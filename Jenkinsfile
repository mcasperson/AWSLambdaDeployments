pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                powershell "Octo pack --format zip --id AwsLambdaHello --version 0.0.${env.BUILD_NUMBER}"
            }
        }

        stage ('Deploy to Octopus') {
            steps {
                withCredentials([string(credentialsId: 'OctopusAPIKey', variable: 'APIKey')]) {
                    powershell """
                        Octo push --package AwsLambdaHello0.0.${env.BUILD_NUMBER}.zip --replace-existing --server ${OctopusServer} --apiKey ${APIKey}
                        Octo create-release --project "Aws Lambda Hello" --server ${OctopusServer} --apiKey ${APIKey}
                        Octo deploy-release --project "Aws Lambda Hello" --version latest --deployto Integration --server ${OctopusServer} --apiKey ${APIKey}
                    """
                }
            }
        }
    }
}