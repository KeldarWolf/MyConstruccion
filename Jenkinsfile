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
                    echo 'Iniciando la construcción del archivo WAR...'
                    if (isUnix()) {
                        sh 'mvn clean package'
                    } else {
                        bat 'mvn clean package'
                    }
                }
            }
        }

        stage('Verify WAR') {
            steps {
                script {
                    echo 'Verificando si el archivo WAR fue generado...'
                    def warFile = 'target/MyConstruction-exa2-0.0.1-SNAPSHOT.war'
                    if (!fileExists(warFile)) {
                        error "El archivo WAR no fue generado en la ubicación esperada: ${warFile}"
                    } else {
                        echo "El archivo WAR fue generado exitosamente: ${warFile}"
                    }
                }
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    echo 'Publicando el archivo WAR a Artifactory...'
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


