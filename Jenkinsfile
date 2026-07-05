pipeline {
    agent any
     
     environment {
         SCANNER_HOME = tool 'sonar-scanner'
          }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Chakrinitesh/sonar-python-project.git'
            }
        }
        stage('Install EVEN & dependencies ') {
            steps {
                sh '''
                    # Remove any existing virtual environments
                    rm -rf venv
                    # Create a new virtual environment
                    python3 -m venv venv
                    chmod -R 755 venv
                    # Activate the virtual environment andinstall dependencies
                    bash -c "
                    source venv/bin/activate && \
                    pip install --upgrade pip && \
                    pip install --upgrade pip setuptools wheel && \
                    pip install -r requirements.txt
                    "
                   '''
            }
        }
        stage('Test') {
            steps {
                sh ''' 
                     . venv/bin/activate 
                     pytest --cov=app --cov-report=xml --cov-report=term-missing --disable-warnings
                       
                   '''
            }
        }
        stage ('Sonar') {
              steps {
                  withSonarQubeEnv('sonarqube'){
                      sh '''
                            $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectKey=Python-project \
                            -Dsonar.projectName=Pythonproject \
                            -Dsonar.exclusions=venv/** \
                            -Dsonar.sources=. \
                            -Dsonar.python.coverage.reportPaths=coverage.xml
                          '''    
                  }
              }
          }
      }
}
