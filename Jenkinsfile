#!Jenkinsfile

node("docker") {

    stage("Checkout") {
        checkout scm
        docker.image("maven:3.3.9-jdk-8").pull()
    }

    stage("Download Build Tools") {
        sh "wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar"
    }

    stage("Prepare Projects") {
        docker.image("maven:3.3.9-jdk-8").inside {
            java -jar BuildTools.jar
        }
    }

}
