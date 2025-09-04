pipeline {
    agent any

    environment {
        // Si no agregaste JMeter al PATH, usamos la ruta completa
        JMETER_PATH = "C:\\apache-jmeter-5.7\\bin\\jmeter.bat"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Clonar el repositorio desde GitHub
                git branch: 'main', url: 'https://github.com/gnieva2024/pruebajmeter'
            }
        }

        stage('Ejecutar Prueba JMeter') {
            steps {
                // Ejecutar JMeter en modo no GUI (-n), con archivo .jmx, guardar resultados y generar reporte
                bat "\"${env.JMETER_PATH}\" -n -t JMeter-Test.jmx -l resultados.jtl -e -o reportes"
            }
        }

        stage('Publicar Reporte JMeter') {
            steps {
                // Archivar los resultados y reporte
                archiveArtifacts artifacts: 'resultados.jtl', fingerprint: true
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reportes',
                    reportFiles: 'index.html',
                    reportName: 'Reporte JMeter'
                ])
            }
        }
    }

    post {
        success {
            echo 'Pipeline finalizado correctamente.'
        }
        failure {
            echo 'Hubo un error en el pipeline.'
        }
    }
}
