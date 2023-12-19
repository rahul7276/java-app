@Library('my-shared-library') _

pipeline{
  agent any
  stages{
    stage('Git Checkout'){
        steps{
            gitCheckout(
                branch: "master",
                url: "https://github.com/vikash-kumar01/mrdevops_java_app.git"
        )
      }
    }
  }
}  
