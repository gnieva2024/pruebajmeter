pipeline {
    agent any

    environment {
        // Ruta completa a jmeter.bat en tu PC
        JMETER_PATH = "C:\\Users\\Graciela\\Downloads\\Apache jmeter\\apache-jmeter-5.6.3\\bin\\jmeter.bat"
        // Nombre del archivo .jmx dentro del repo
        JMETER_TEST = "testplan.jmx"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Clonar el repositorio
                git branch: 'main', url: 'https://github.com/gnieva2024/pruebajmeter'
            }
        }

        stage('Ejecutar Prueba JMeter') {
            steps {
                // Verificar que el archivo .jmx exista
                bat """
                if exist ${JMETER_TEST} (
                    echo Archivo .jmx encontrado: ${JMETER_TEST}
                    "${JMETER_PATH}" -n -t ${JMETER_TEST} -l resultados.jtl -e -o reportes
                ) else (
                    echo ERROR: No se encontró el archivo ${JMETER_TEST}
                    exit /b 1
                )
                """
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
