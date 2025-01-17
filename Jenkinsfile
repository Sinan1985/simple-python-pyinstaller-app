pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                bat 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Setup') {
            steps {
                echo 'Setting up...'
                bat 'pip install pytest pyinstaller'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
                junit 'test-reports/results.xml'
            }
        }
        stage('Deliver') {
            steps {
                echo 'Delivering...'
                bat 'if not exist dist\\add2vals mkdir dist\\add2vals'
                bat 'pyinstaller --onefile sources/add2vals.py'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }
}
