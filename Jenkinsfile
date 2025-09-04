pipeline {
    agent any

    environment {
        // Ruta a jmeter.bat en tu PC
        JMETER_PATH = "C:\\Users\\Graciela\\Downloads\\Apache jmeter\\apache-jmeter-5.6.3\\bin\\jmeter.bat"
        // Archivo de prueba dentro del repo
        JMETER_TEST = "testplan.jmx"
        // Carpeta de salida para resultados y reportes
        OUTPUT_DIR = "reporte_nuevo"
        RESULT_FILE = "resultados.jtl"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/gnieva2024/pruebajmeter'
            }
        }

        stage('Preparar Workspace') {
            steps {
                bat """
                REM Borrar resultados anteriores si existen
                if exist ${OUTPUT_DIR} rmdir /s /q ${OUTPUT_DIR}
                if exist ${RESULT_FILE} del /f /q ${RESULT_FILE}
                """
            }
        }

        stage('Ejecutar Prueba JMeter') {
            steps {
                bat """
                REM Verificar que el archivo .jmx exista
                if exist ${JMETER_TEST} (
                    echo Archivo .jmx encontrado: ${JMETER_TEST}
                    "${JMETER_PATH}" -n -t ${JMETER_TEST} -l ${RESULT_FILE} -e -o ${OUTPUT_DIR}
                ) else (
                    echo ERROR: No se encontró el archivo ${JMETER_TEST}
                    exit /b 1
                )
                """
            }
        }

        stage('Publicar Reporte JMeter') {
            steps {
                archiveArtifacts artifacts: "${RESULT_FILE}", fingerprint: true
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: OUTPUT_DIR,
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
