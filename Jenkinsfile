pipeline {
  agent any
  parameters {
    string(name: 'container_name', defaultValue: 'project_test', description: 'docker name')
    string(name: 'image_name', defaultValue: 'ip_project_test', description: 'name image')
    string(name: 'image_tag', defaultValue: 'latest', description: 'docker version')
    string(name: 'imagen_port', defaultValue: '8094', description: 'Public port')
  }
  environment {
    final_name = "${container_name}"
  }
  stages {
    stage('stop/rm') {
      when {
        expression {
          DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=${final_name})"').trim()
          return DOCKER_EXIST != ''
        }
      }
      steps {
        script {
          sh '''
            docker stop ${final_name}
          '''
        }
      }
    }
    stage('build') {
      steps {
        script {
          sh '''
            docker build web/ . -t ${image_name}:${image_tag}
          '''
        }
      }
    }
    stage('run') {
      steps {
        script {
          sh '''
            docker run -dp ${imagen_port}:8094 --name ${final_name} ${image_name}:${image_tag}
          '''
        }
      }
    }
  }
}
