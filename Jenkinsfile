pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                bat 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
	stage('Setup') {
		steps {
			bat 'pip install pytest pyinstaller'
		}
	}
	stage('Test') {
            steps {
                bat 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
	stage('Deliver') {
            steps {
				// Create the directory if it doesn't exist
				bat 'if not exist dist\\add2vals mkdir dist\\add2vals'
				
                bat "pyinstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}