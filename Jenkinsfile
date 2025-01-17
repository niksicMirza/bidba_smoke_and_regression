pipeline {
    agent any
    tools
    {
      maven 'Maven'
    }
    stages
    {
      stage('Checkout')
      {
        steps
        {
          deleteDir()
          git url:'https://github.com/niksicMirza/ABH-Internship-BidBa-Testing.git'
        }
      }
      stage('Smoke Test')
      {
        steps
        {
          echo "Test"
          bat 'mvn test -DsuiteXmlFile="smoke.xml"'
        }
      }
      stage('Report Smoke') {
        steps {
          script {
            allure([
                    includeProperties:false,
                    jdk:'',
                    properties: [],
            reportBuildPolicy:
            'ALWAYS',
                    results: [[path:
            'target/allure/allure-results']]
              ])
          }
          }
        }
        stage("Zip Report File Smoke"){
          steps{
            script{
            zip zipFile: 'smokeTest.zip', archive: false, dir: 'target/allure'
              }
          }
        }
        stage('Regression Test')
          {
            steps
            {
              echo "Test"
              bat 'mvn  test -DsuiteXmlFile="regression.xml"'
            }
          }
      stage('Report Regression') {
      steps {
        script {
          allure([
                  includeProperties:false,
                  jdk:'',
                  properties: [],
          reportBuildPolicy:
          'ALWAYS',
                  results: [[path:
          'target/allure/allure-results']]
            ])
        }
        }
      }
      stage("Zip Report File Regression"){
          steps{
        script{
        zip zipFile: 'regressionTest.zip', archive: false, dir: 'target/allure'
          }
      }
    }
    }
        post {
      success {
        bat "echo 'Send mail on success'"
       emailext attachmentsPattern: 'smokeTest.zip, regressionTest.zip',attachLog: true, body: "Tests Passed", mimeType: 'text/html', subject: 'Passed', to: 'sssdprojekat@gmail.com', from:'jenkinsApiSmoke@gmail.com'
      }
      failure {
        bat "echo  'Send mail on failure'"
        emailext attachmentsPattern: 'smokeTest.zip, regressionTest.zip',attachLog: true, body: "Tests Failed", mimeType: 'text/html', subject: 'Failed', to: 'sssdprojekat@gmail.com', from:'jenkinsApiSmoke@gmail.com'
      }
    }
  }
