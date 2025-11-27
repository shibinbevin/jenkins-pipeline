pipeline {
    agent any
    environment {
        REPORTS_DIR = 'reports'
        JMX_FILE = 'Test-Plan.jmx'
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Fetch your JMeter test plan from a SCM (e.g., Git)
                git 'https://github.com/shibinbevin/jenkins-pipeline.git' 
            }
        }
        stage('Run JMeter Test') {
            steps {
                script {
                    bat "jmeter -n -t ${JMX_FILE} -l ${REPORTS_DIR}/results.jtl -e -o ${REPORTS_DIR}/html-report"
                }
            }
        }
        stage('Publish Performance Report') {
            steps {
                script {
                    if (fileExists("${REPORTS_DIR}/results.jtl")) {
                        performanceReport parsers: [[parser: 'JMeterCSV', glob: "${REPORTS_DIR}/results.jtl"]]
                    } else {
                        echo "JMeter results file not found: ${REPORTS_DIR}/results.jtl"
                    }
                }
            }
        }
        // stage('Archive Artifacts') {
        //     steps {
        //         // Archive the generated .jtl and HTML reports
        //         archiveArtifacts artifacts: "${REPORTS_DIR}/**", allowEmptyArchive: true
        //     }
        // }
    }
}