pipeline {
    agent any

    stages {
        stage('Database Maintenance') {
            steps {
                script {
                    // Descargar la base de datos
                    sh 'wget -O Employees.db https://github.com/rubenixter/PROF-2023-Ejercicio4/blob/main/Employees.db'

                    // Hacer una copia de los datos actuales
                    sh 'sqlite3 Employees.db ".dump" > backup.sql'
                
                    // Cargar el nuevo esquema borrando la db
                    sh 'rm Employees.db'
                    sh 'sqlite3 Employees.db < sqlite.sql'

                    // Restaurar los datos respaldados anteriormente
                    sh 'sqlite3 Employees.db < backup.sql'
                }
            }
        }
    }

    post {
        success {
            // Publicar un estado de éxito en GitHub
            script {
                currentBuild.result = 'SUCCESS'
                echo 'Database maintenance successful!'
                updateGitHubStatus context: 'Database Maintenance', description: 'Success', state: 'SUCCESS'
            }
        }
        failure {
            // Publicar un estado de fallo en GitHub
            script {
                currentBuild.result = 'FAILURE'
                echo 'Database maintenance failed!'
                updateGitHubStatus context: 'Database Maintenance', description: 'Failed', state: 'FAILURE'
            }
        }
    }
}
