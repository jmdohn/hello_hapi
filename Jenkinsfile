node {
    stage ('Checkout') {
        checkout scm
    }
    
    stage ('Run Analysis'){
        sh './pmd-bin-6.36.0/bin/run.sh pmd -d . -R ./rulesets/pmd.xml -f xml -l apex -r target/pmd.xml'
    }

    stage ('Build and Static Analysis') {
        recordIssues tools: [
            pmdParser(pattern: 'target/pmd.xml'),
            cpd(pattern: 'target/cpd.xml')],
            qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]
        ]
    }
}
