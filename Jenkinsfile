@Library('my-shared-library') _

pipeline{
  agent any
  stages{
    stage('Git Checkout'){
        steps{
            gitCheckout(
                branch: "master",
                url: "https://github.com/rahul7276/java-app.git"
        )
      }
    }
  }
}  
