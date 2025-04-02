pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Usar checkout scm en lugar de git url
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Instalar dependencias con npm
                bat 'npm install'
            }
        }
        
        stage('Start Server') {
            steps {
                // Iniciar el servidor en segundo plano
                bat 'start /B npm start'
                // Esperar a que el servidor esté listo usando ping en lugar de timeout
                bat 'ping -n 10 127.0.0.1 > nul'
            }
        }
        
        stage('Run Tests') {
            steps {
                // Ejecutar pruebas y generar reporte
                bat 'npm run test:report'
            }
            post {
                always {
                    // Guardar los resultados de las pruebas
                    junit 'junit.xml'
                }
                failure {
                    echo 'Las pruebas han fallado!'
                }
                success {
                    echo 'Las pruebas han sido exitosas!'
                }
            }
        }
        
        stage('Generate Report') {
            steps {
                // Crear archivo REPORT.md
                bat '''
                @echo off
                echo # Reporte de Ejecución del Pipeline > REPORT.md
                echo. >> REPORT.md
                echo ## Pasos Realizados >> REPORT.md
                echo. >> REPORT.md
                echo 1. Se clonó el repositorio usando checkout scm >> REPORT.md
                echo 2. Se instalaron las dependencias con npm install >> REPORT.md
                echo 3. Se inició el servidor para validar la API >> REPORT.md
                echo 4. Se ejecutaron las pruebas automatizadas >> REPORT.md
                echo 5. Se generó este reporte con los resultados >> REPORT.md
                echo. >> REPORT.md
                echo ## Resultados de las Pruebas >> REPORT.md
                echo. >> REPORT.md
                echo Los resultados detallados de las pruebas se pueden encontrar en el reporte JUnit. >> REPORT.md
                echo. >> REPORT.md
                echo ## Problemas Encontrados >> REPORT.md
                echo. >> REPORT.md
                if errorlevel 1 (
                    echo - Se encontraron errores durante la ejecución de las pruebas. >> REPORT.md
                ) else (
                    echo - No se encontraron problemas durante la ejecución. >> REPORT.md
                )
                '''
                
                // Archivar el reporte
                archiveArtifacts artifacts: 'REPORT.md', fingerprint: true
            }
        }
    }
    
    post {
        always {
            // Detener el servidor al finalizar
            bat 'taskkill /F /IM node.exe || exit 0'
        }
    }
}
