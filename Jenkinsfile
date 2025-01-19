pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/KeldarWolf/MyConstruccion.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Ejecutando Maven para construir el proyecto'
                // Cambiar `sh` por `bat` para Windows
                bat 'mvn clean package'
            }
        }
        stage('Archive WAR') {
            steps {
                echo 'Archivando el archivo WAR generado'
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }
    post {
        failure {
            echo 'La construcción falló. Revisa los logs de errores.'
        }
    }
}


