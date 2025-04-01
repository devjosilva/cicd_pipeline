pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clonar el código desde el repositorio remoto
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Instalar dependencias
                bat 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                // Ejecutar pruebas y hacer que el pipeline falle si las pruebas fallan
                bat 'npm test'
            }
        }
        
        stage('Start Server') {
            steps {
                // Iniciar el servidor para validar que la API responde
                bat 'start /b npm start'
                // Dar tiempo para que el servidor inicie
                bat 'timeout /t 5'
            }
        }
        
        stage('Verify API') {
            steps {
                // Verificar que la API responde correctamente
                bat 'curl -f http://localhost:3000/users'
            }
        }
        
        stage('Generate Test Report') {
            steps {
                // Generar informe de pruebas
                bat 'npm run test:report'
            }
            post {
                always {
                    // Publicar resultados de pruebas
                    junit '**/test-results.xml'
                }
            }
        }
    }
    
    post {
        always {
            // Detener el servidor después de las pruebas
            bat 'taskkill /f /im node.exe'
        }
        success {
            echo 'Pipeline ejecutado correctamente!'
        }
        failure {
            echo 'El pipeline ha fallado. Revisar los logs para más detalles.'
        }
    }
}
