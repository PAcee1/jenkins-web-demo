pipeline {
   agent any

   stages {
      stage('pull code') {
         steps {
            echo 'pull code'
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'b54233ca-0569-427c-b82a-aad8dcbd0714', url: 'git@github.com:PAcee1/jenkins-web-demo.git']]])
         }
      }
      stage('check code') {
           steps {
              echo 'check code'
              script {
                    // 引入SonarQube Scanner工具，这里是我们在Jenkins全局工具中配置的名称
                    scannerHome = tool 'sonar-scanner'
              }
              // 引入Sonar服务器环境，这里是我们Jenkins系统配置中Sonar的名称
              withSonarQubeEnv('sonar'){
                    sh "${scannerHome}/bin/sonar-scanner"
              }
           }
        }
      stage('build code') {
         steps {
            echo 'build code'
            sh label: '', script: 'mvn clean package'
         }
      }
      stage('publish code') {
         steps {
            echo 'publish code'
            deploy adapters: [tomcat8(credentialsId: '068f0e24-9c42-4c85-a83c-2f22210cffd9', path: '', url: 'http://192.168.56.132:8080/')], contextPath: null, war: 'target/*.war'
         }
      }
   }
   post {
    always {
        emailext(
            subject: '构建通知: ${PROJECT_NAME} - Build # ${BUILD_NUMBER} - ${BUILD_STATUS}!',
            body: '${FILE, path="email.html"}',
            to: '8709867@qq.com'
        )
    }
   }
}
