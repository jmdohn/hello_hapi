pipeline{
    agent any
    stages {
        stage ('Checkout') {
            steps{
                checkout scm
            }
        }
        
        stage('Run PMD') {
            steps{
                sh './pmd-bin-6.36.0/bin/run.sh pmd -d . -R ./rulesets/pmd.xml -f xml -r target/pmd.xml'
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
