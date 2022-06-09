pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('pre-build') {
            parallel{
                stage('depedency-check') {
                    steps {
                        sh 'rm owasp* || true'
                        sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
                        sh 'chmod +x owasp-dependency-check.sh'
                        sh 'bash owasp-dependency-check.sh'
                        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
                    }
                }
                stage('secret-check') {
                    steps {
                        sh 'rm trufflehog || true'
                        sh 'docker run --rm gesellix/trufflehog --json https://github.com/tuanklnew/BenchmarkJava.git |& tee trufflehog'
                        sh 'cat trufflehog'
                    }
                }
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }

    }
}
