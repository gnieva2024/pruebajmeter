pipeline {
    agent any

    environment {
        // Ruta correcta a jmeter.bat
        JMETER_PATH = "C:\\Users\\Graciela\\Downloads\\Apache jmeter\\apache-jmeter-5.6.3\\bin\\jmeter.bat"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/gnieva2024/pruebajmeter'
            }
        }

        stage('Ejecutar Prueba JMeter') {
            steps {
                // Ejecutar JMeter con archivo .jmx
                bat "\"${env.JMETER_PATH}\" -n -t JMeter-Test.jmx -l resultados.jtl -e -o reportes"
            }
        }

        stage('Publicar Reporte JMeter') {
            steps {
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
