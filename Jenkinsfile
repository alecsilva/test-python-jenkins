pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                // Instalar las dependencias requeridas usando pip
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Build') {
            steps {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') { 
            steps {
                sh 'python3 -m pytest --junit-xml test-reports/results.xml sources/test_calc.py' 
            }
            post {
                always {
                    junit 'test-reports/results.xml' 
                }
            }
        }
        stage('Deliver') {
            steps {
                sh "python3 -m PyInstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
