pipeline {
    agent any

    environment {
        ARTIFACTORY_USR = 'admin'  // Usuario de Artifactory
        ARTIFACTORY_PSW = 'Kevin023'  // Contraseña de Artifactory
        ARTIFACTORY_URL = 'http://localhost:8082/artifactory'  // URL de Artifactory
        ARTIFACTORY_REPO = 'mvn'  // Repositorio en Artifactory
    }

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

        stage('Publish to Artifactory') {
            steps {
                script {
                    // Verificar si el archivo WAR se generó en la ubicación correcta
                    echo 'Publicando WAR a Artifactory...'

                    // Subir el archivo WAR a Artifactory (asegurarse de que el nombre y ruta sean correctos)
                    sh """
                    curl -u ${env.ARTIFACTORY_USR}:${env.ARTIFACTORY_PSW} -X PUT -T target/MyConstruction-exa2-0.0.1-SNAPSHOT.war ${env.ARTIFACTORY_URL}/${env.ARTIFACTORY_REPO}/MyConstruction-exa2-0.0.1-SNAPSHOT.war
                    """
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
            echo '¡Construcción completada exitosamente y desplegada a Artifactory!'
        }
        failure {
            echo 'La construcción o el despliegue fallaron. Revisa los logs de errores.'
        }
    }
}

