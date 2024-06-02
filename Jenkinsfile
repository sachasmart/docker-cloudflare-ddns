pipeline {
  options {
    ansiColor('xterm')
    }

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
}

  stages {
    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                            --context `pwd` \
                            --custom-platform=linux/amd64 \
                            --destination=push-registry.sachasmart.com/cloudflare-ddns:${BUILD_NUMBER} \
                            --cache=true
            '''
          }
        }
    }
    post {
        success {
            discordSend description: 'Cloudflare DDNS Container Update',
              title: env.JOB_NAME,
              footer: env.JOB_URL,
              image: '',
              result: 'SUCCESS',
              scmWebUrl: '',
              thumbnail: '',
              webhookURL: 'https://discord.com/api/webhooks/1151647533068718110/W1do5iFTAvMK8jaA65pDoL5MtnnV0K46lAiJbRS2NnZoqOfmR4i1t4fqsYNBM3ot3OE-'
        }
      }
    }
  }
}