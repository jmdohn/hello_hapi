node {
    stage ('Checkout') {
        checkout scm
    }
    
    stages ('Run Analysis'){
        stage('Check PMD') {
            when{
                expression { sh 'test -d pmd-bin-6.36.0 && echo true || echo false' == false}
            }
            steps {
                sh 'curl -L "https://github.com/pmd/pmd/download/pmd_releases%2F6.36.0/pmd-bin-6.36.0.zip" -o pmd-bin-6.36.0.zip'
                sh 'unzip pmd-bin-6.36.0.zip'
                sh 'rm pmd-bin-6.36.0.zip'
            }
        }
        stage('Run PMD') {
            sh './pmd-bin-6.36.0/bin/run.sh pmd -d . -R ./rulesets/pmd.xml -f xml -l apex -r target/pmd.xml'
        }
    }

    stage ('Build and Static Analysis') {
        recordIssues tools: [
            pmdParser(pattern: 'target/pmd.xml'),
            cpd(pattern: 'target/cpd.xml')],
            qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]
        ]
    }
}
