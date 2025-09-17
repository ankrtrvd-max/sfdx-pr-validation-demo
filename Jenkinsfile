pipeline {
    agent any

    environment {
        SFDX_AUTH_URL = credentials('sfdx-auth-url')
        SCRATCH_ORG_ALIAS = "PR_Validation_Org"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SFDX Auth') {
            steps {
                sh """
                    echo $SFDX_AUTH_URL > auth-url.txt
                    sfdx auth:sfdxurl:store -f auth-url.txt -a DevHub --setdefaultdevhubusername
                """
            }
        }

        stage('Validate Deployment') {
            steps {
                sh """
                    sfdx force:org:create -f config/project-scratch-def.json -a $SCRATCH_ORG_ALIAS -s -d 1
                    sfdx force:source:push -u $SCRATCH_ORG_ALIAS --forceoverwrite
                    sfdx force:apex:test:run -u $SCRATCH_ORG_ALIAS --resultformat human --wait 10 --codecoverage
                """
            }
        }
    }

    post {
        success {
            echo "Validation succeeded"
        }
        failure {
            mail to: 'ankur.trivedi@salesforce.com',
                 subject: "PR Validation Failed in Jenkins",
                 body: "PR validation failed for build ${env.BUILD_NUMBER}. Check logs in Jenkins."
        }
    }
}
