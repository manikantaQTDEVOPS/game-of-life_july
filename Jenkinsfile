pipeline {
      // label is set for node1
      agent { label 'JDK17' }
      // job is stop after 30min
      options { 
            retry(3)
            timeout(time: 30, unit: 'MINUTES' )
      }
      // jenkins ask the git for every new commit at every minute
      triggers {
            pollSCM('* * * * *')
      }
      // java-17 and maven-3.6.3 tools required for this project
      tools {
            maven 'maven-default'
            jdk 'java-8'     
      }

      // added parameters to the maven goal
      parameters { choice(name: 'GOAL' , choices: [ 'package', 'clean package',
      'install', 'clean install']

      )

      }
      // source code from the version control system
      stages {
            stage('source code') {
                  steps {
                        git url: 'https://github.com/manikantaQTDEVOPS/game-of-life_july.git',
                            branch: 'master'
                  }
            }
      //build and package the code using maven

            stage('build and package') {
                  steps {
                        sh script: "mvn ${params.GOAL}"
                  }
            }

     //archive the artifacts and run the unit tests

            stage('reporting') {
                  steps {
                        archiveArtifacts artifacts: '**/target/gameoflife.war'
                        junit testResults: '**/surefire-reports/TEST-*.xml'
                  }
            }  
            
      }
      // send mails about the project whether it is success or not
      post {
            success {
                  mail subject: 'your project is effective',
                        body: 'your project is affective',
                        to: 'rajesh@qt.com'
            }
            failure {
                  mail subject: 'your project is diffective',
                       body: 'your project is diffective',
                       to: 'rajesh@qt.com'
            }
      }

}