pipeline {
    agent any

    stages {
        stage('Hello3') {
            steps {
                echo 'Hello World21'
                    // Clonamos el repositorio desde GitHub
                git url: 'https://github.com/erixrivas/draft-proyectofinal.git', credentialsId: 'githuberix', branch: 'folder-refactor'
                 sh """
                 ls 
                 cd draft-proyectofinal/k8s/dev/api
                 ls
                 """
            }
        }
    }
}