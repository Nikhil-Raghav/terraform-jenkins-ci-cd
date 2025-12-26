pipeline {
    parameters {
        choice(
            name: 'terraformAction',
            choices: ['apply', 'destroy'],
            description: 'Choose your Terraform action to perform'
        )
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any

    stages {

        stage('Checkout') {
            steps {
                script {
                    dir("iac-repo") {
                        git branch: 'main',
                            url: 'https://github.com/<your-username>/<your-terraform-repo>.git'
                    }
                }
            }
        }

        stage('Plan') {
            steps {
                sh 'cd iac-repo/terraform-project; terraform init'
                sh 'cd iac-repo/terraform-project; terraform plan -out tfplan'
                sh 'cd iac-repo/terraform-project; terraform show -no-color tfplan >> ../tfplan.txt'
            }
        }

        stage('Approval') {
            steps {
                script {
                    def plan = readFile 'iac-repo/tfplan.txt'
                    input(
                        message: 'Do you want to proceed with the Terraform action?',
                        parameters: [
                            text(
                                name: 'planReview',
                                description: 'Review the Terraform plan before approval',
                                defaultValue: plan
                            )
                        ]
                    )
                }
            }
        }

        stage('Apply or Destroy') {
            when {
                expression {
                    params.terraformAction == 'apply' || params.terraformAction == 'destroy'
                }
            }

            steps {
                script {
                    if (params.terraformAction == 'apply') {
                        sh 'cd iac-repo/terraform-project; terraform apply -input=false tfplan'
                    } else {
                        sh 'cd iac-repo/terraform-project; terraform destroy -auto-approve'
                    }
                }
            }
        }
    }
}
