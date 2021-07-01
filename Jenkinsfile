pipeline{
    agent any
    stages {
        stage ('Checkout') {
            steps{
                checkout scm
            }
        }
        
        stage('Check PMD') {
            when{
                expression { sh 'test -d pmd-bin-6.36.0 && echo "false" || echo "true"' == 'true'}
            }
            steps {
                sh 'curl -L "https://github.com/pmd/pmd/download/pmd_releases%2F6.36.0/pmd-bin-6.36.0.zip" -o pmd-bin-6.36.0.zip'
                sh 'unzip pmd-bin-6.36.0.zip'
                sh 'rm pmd-bin-6.36.0.zip'
            }
        }
        
        stage('Run PMD') {
            steps{
                sh './pmd-bin-6.36.0/bin/run.sh pmd -d . -R ./rulesets/pmd.xml -f xml -l apex -r target/pmd.xml'
            }
        }
        
        stage ('Build and Static Analysis') {
            steps{
                recordIssues tools: [
                pmdParser(pattern: 'target/pmd.xml'),
                cpd(pattern: 'target/cpd.xml')],
                qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]
            ]
            }
        }
    }
}
