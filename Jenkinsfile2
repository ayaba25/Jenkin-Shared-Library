// def COLOR_MAP = [
//     'SUCCESS': 'good', 
//     'FAILURE': 'danger',
// ]
pipeline {
  agent any
  environment {
    WORKSPACE = "${env.WORKSPACE}"
  }
  tools {
    maven 'localmaven'
   // jdk 'localJdk'
  }
  stages {
    stage('Get Code from Github') {
        steps {
        git branch: 'main', url: 'https://github.com/awanmbandi/eagles-batch-devops-projects.git'
      }
      }
      
    stage('Build') {
        steps {
        sh 'mvn clean package'
      }
    }
    
    stage('Unit Test'){
        steps {
            sh 'mvn test'
        }
    }
    
    stage('Integration Test'){
        steps {
          sh 'mvn verify -DskipUnitTests'
        }
    }
    stage ('Checkstyle Code Analysis'){
        steps {
            sh 'mvn checkstyle:checkstyle'
        }

    }
    stage('SonarQube Scan') {
        steps {
        sh """mvn sonar:sonar \
  -Dsonar.projectKey=javaWeb \
  -Dsonar.host.url=http://172.31.12.99:9000/ \
  -Dsonar.login=87f9730b20ac159866d1a40bfd9ce5428fad7906"""
      }
    }
	
	
    stage('Upload to Artifactory') {
      steps {
        sh "mvn clean deploy -DskipTests"
      }
    }
    
    // stage('Deploy to DEV') {
    //   environment {
    //     HOSTS = "dev"
    //   }
    //   steps {
    //     sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
    //   }
    //  }
    // stage('Approval for stage') {
    //   steps {
    //     input('Do you want to proceed?')
    //   }
    // }
//     stage('Deploy to Stage') {
//       environment {
//         HOSTS = "stage" // Make sure to update to "stage"
//       }
//       steps {
//         sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
//       }
//     }
//     stage('Approval') {
//       steps {
//         input('Do you want to proceed?')
//       }
//     }
//     stage('Deploy to PROD') {
//       environment {
//         HOSTS = "prod"
//       }
//       steps {
//         sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
//       }
//     }
  }
}
//   post {
//     always {
//         echo 'Slack Notifications.'
//         slackSend channel: '#mbandi-jenkins-cicd-pipeline-alerts', //update and provide your channel name
//         color: COLOR_MAP[currentBuild.currentResult],
//         message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
//     }
//   }
