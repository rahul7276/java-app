@Library('my-shared-library') _

pipeline{
  agent any
  parameters{
    choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
    //string(name: 'aws_account_id', description: " AWS Account ID", defaultValue: '121797143523')
    //string(name: 'Region', description: "Region of ECR", defaultValue: 'ap-south-1')
    // string(name: 'ECR_REPO_NAME', description: "name of the ECR", defaultValue: 'rahul0403')
 }
  stages{
    stage('Git Checkout'){
      when { expression {  params.action == 'create' } }
      steps{
        gitCheckout(
          branch: "master",
          url: "https://github.com/rahul7276/java-app.git"
        )
      }
    }
    stage('Unit Test maven'){
      when { expression {  params.action == 'create' } }
      steps{
        script{
          mvnTest()
        }
      }
    }
    stage('Integration Test Maven'){
      when { expression {  params.action == 'create' } }
      steps{
        script{
          mvnIntegrationTest()
        }
      }        
    }
    stage('Static code analysis: Sonarqube'){
      when { expression {  params.action == 'create' } }
        steps{
          script{
            def SonarQubecredentialsId = 'sonarqube-api'
            statiCodeAnalysis(SonarQubecredentialsId)
          }
        }
      }
    stage('Quality Gate Status Check : Sonarqube'){
      when { expression {  params.action == 'create' } }
        steps{
          script{
            def SonarQubecredentialsId = 'sonarqube-api'
            QualityGateStatus(SonarQubecredentialsId)
          }
        }
      }
    stage('Maven Build'){
      when { expression {  params.action == 'create' } }
        steps{
          script{
            mvnBuild()
          }
        }
      }
    stage('Docker Image Build : ECR'){
      when { expression {  params.action == 'create' } }
        steps{
          script{
            //dockerBuild("${params.aws_account_id}","${params.Region}","${params.ECR_REPO_NAME}")
            aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 121797143523.dkr.ecr.ap-south-1.amazonaws.com
            docker build -t rahul0403 .
            docker tag rahul0403:latest 121797143523.dkr.ecr.ap-south-1.amazonaws.com/rahul0403:latest
            docker push 121797143523.dkr.ecr.ap-south-1.amazonaws.com/rahul0403:latest
          }
        }
      }    
    }
  }
