pipeline {
  agent any

	stages {
		stage('Build') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
					ls -la
					node -v
					npm -v
					npm ci
					npm run build
					ls -la
				'''
			}
		}

		stage('Test') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
					test -f build/index.html
					npm test
				'''
			}
			post {
				always {
					junit 'jest-results/junit.xml'
				}
			}
		}

		stage('Deploy') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
					npm install netlify-cli
					node_modules/.bin/netlify --version
				'''
			}
		}

		/*
		stage('E2E Tests') {
			agent {
				docker {
					image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
					reuseNode true
				}
			}
			steps {
				sh '''
					npm install serve
					node_modules/.bin/serve -s build &
					sleep 5
					npx playwright test
				'''
			}
		}
		*/
	}
}
