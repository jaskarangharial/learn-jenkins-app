pipeline {
  agent {
    docker {
      image 'node:18-alpine'
      reuseNode true
    }
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
    }

    stage('Deploy') {
      steps {
        sh '''
          echo "Deploying"
          npm install netlify-cli
          node_modules/.bin/netlify --version
        '''
      }
    }
  }
  post {
    always {
      junit 'test-results/junit.xml'
    }
  }
}
