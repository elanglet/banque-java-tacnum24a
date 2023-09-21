pipeline {
    agent any

    tools {
        // Déclarer et potentiellement installer les outils nécessaire au build.
        // On utilise les noms déclarés dans la section "Tools"
        maven "MVN3"
        jdk "JDK11"
    }

    stages {
        stage('SCM') {
            steps {
                // Récupérer le code depuis le SCM
                git url: "https://github.com/elanglet/banque-portail-java", branch: "main"
            }
        }
        stage('Build') {
            steps {
                // On spécifie la version de Maven et de Java à utiliser pour le build
                withMaven(maven: 'MVN3', jdk: 'JDK11') {
                    // Lancer la commande Maven pour packager l'application
                    bat "mvn -f banque/pom.xml package"
                }
            }
            post {
                // Etape après le build Maven. Si le build est en succès, alors 
                // On publie les résultats de tests JUnit
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Déploiement Docker') {
            steps {
                bat ".\\docker\\pre-deploy.cmd"
                bat "docker compose -f docker\\compose.yaml up -d --build"
            }
        }
    }
}
