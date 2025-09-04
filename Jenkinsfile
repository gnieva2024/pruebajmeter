pipeline {
    agent any

    stages {
        stage('Checkout Código') {
            steps {
                git branch: 'main', url: 'https://github.com/gnieva2024/pruebajmeter'
            }
        }

        stage('Ejecutar Prueba JMeter') {
            steps {
                // Ejecuta el test plan
                bat '"C:\\apache-jmeter-5.7\\bin\\jmeter.bat" -n -t JMeter-Test.jmx -l resultados.jtl -e -o reportes'
            }
        }

        stage('Publicar Reporte JMeter') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reportes',
                    reportFiles: 'index.html',
                    reportName: 'JMeter Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'resultados.jtl', fingerprint: true
        }
    }
}

