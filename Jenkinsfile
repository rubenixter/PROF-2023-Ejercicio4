pipeline {
    agent any

    stages {
        stage('Database Maintenance') {
            steps {
                script {
                    // Descargar la base de datos
                    sh 'wget -O Employees.db https://ruta/al/fichero/Employees.db'

                    // Hacer una copia de los datos actuales
                    sh 'sqlite3 Employees.db ".dump" > backup.sql'

                    // Eliminar el esquema actual
                    sh 'sqlite3 Employees.db < drop.sql'

                    // Cargar el nuevo esquema
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
            }
        }
        failure {
            // Publicar un estado de fallo en GitHub
            script {
                currentBuild.result = 'FAILURE'
                echo 'Database maintenance failed!'
            }
        }
    }
}
