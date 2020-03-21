pipeline {
   agent any

   stages {
      stage('pull code') {
         steps {
            echo 'pull code'
            checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'b54233ca-0569-427c-b82a-aad8dcbd0714', url: 'git@192.168.56.130:pace_group/web-demo.git']]])
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
