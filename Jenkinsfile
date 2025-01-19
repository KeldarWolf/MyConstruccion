pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clonar el repositorio desde la rama 'main'
                git branch: 'main', url: 'https://github.com/KeldarWolf/MyConstruccion.git'
            }
        }

        stage('Build') {
            steps {
                // Construir el proyecto usando Maven
                script {
                    // Verifica si Maven está instalado en Jenkins
                    if (isUnix()) {
                        sh 'mvn clean package'
                    } else {
                        bat 'mvn clean package'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // Ejecutar pruebas con Maven
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
            }
        }

        stage('Archive WAR') {
            steps {
                // Archivar el archivo WAR generado
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '¡Construcción completada exitosamente!'
        }
        failure {
            echo 'La construcción falló. Revisa los logs de errores.'
        }
    }
}
