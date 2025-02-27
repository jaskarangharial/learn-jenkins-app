pipeline {
  agent {
    docker {
      image 'node:18-alpine'
      reuseNode true
    }
  }
  environment {
    NETLIFY_SITE_ID = '8c656609-9411-498e-b7df-9e6e5ea61032'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token')
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          ls -la
          node --version
          npm --version
          npm ci
          npm run build
          ls -la
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          test -f build/index.html
          npm test
        '''
      }
      post {
        always {
          script {
            // Publish JUnit results
            junit 'test-results/junit.xml'
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          npm install netlify-cli
          node_modules/.bin/netlify --version
          node_modules/.bin/netlify link --id $NETLIFY_SITE_ID
          echo "Deploying to Production Site: ${NETLIFY_SITE_ID}"
          node_modules/.bin/netlify status
          node_modules/.bin/netlify deploy --dir=build --prod
        '''
      }
    }
  }
}
