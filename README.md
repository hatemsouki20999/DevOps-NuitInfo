Image docker docker hub : windsocially/app_node_crud


cretion pipeline jenkins 
 pipeline {
    agent any

    stages {
        stage('ecrase l'image docker') {
          steps {
            script {
                                  
                        // L'image Docker contenant le code de l'application web en langage interprété
                        def dockerImage = docker.image('windsocially/app_node_crud')

                        // Créer un conteneur temporaire à partir de l'image Docker
                        def container = dockerImage.run('-d')

                        try {
                            // Copier le code source depuis le conteneur temporaire vers le workspace Jenkins
                            sh "docker cp ${container.id}:/usr/src/app ."
                        } finally {
                            // Arrêter et supprimer le conteneur temporaire
                            container.stop()
                            codeContainer.remove()
                            
                        }
             }     
            }
        }
        stage('Transfert du code vers GitHub') {
            steps {
                script {
                   
                  **** Utiliser Git pour ajouter, valider et pousser le code vers GitHub****
                  
                        withCredentials([usernamePassword(credentialsId: 'username', passwordVariable: 'password', usernameVariable: 'test')]) {    
                       ***clone repository avec ssh || https 
      sh 'git clone git@github.com:hatemsouki20999/DevOps-NuitInfo.git'

       sh 'git clone https:${username}:${password}@github.com/hatemsouki20999/DevOps-NuitInfo.git'      
}

               sh 'cp -r app DevOps-NuitInfo'

                        *** Ajout, commit et push des modifications
                        dir('DevOps-NuitInfo') {
                            sh 'git add .'
                            sh 'git commit -m "Ajout du code extrait"'
                            sh 'git push origin main'
                        }
                      
                }
            }
        }
    }
}
