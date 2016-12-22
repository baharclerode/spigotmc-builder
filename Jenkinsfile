#!Jenkinsfile

node("docker") {

    stage("Checkout") {
        checkout scm
        docker.image("maven:3.3.9-jdk-8").pull()
    }

    stage("Download Build Tools") {
        sh "wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar"
    }

    docker.image("maven:3.3.9-jdk-8").inside {
        withEnv(["HOME=${pwd()}"]) {
            stage("Prepare Projects") {
                sh "java -jar BuildTools.jar --skip-compile --generate-source --generate-docs"
            }
            stage("Build Bukkit") {
                withMaven(globalMavenSettingsConfig: "maven-dragonZone", mavenLocalRepo: "?/.m2/repository") {
                    sh "cd Bukkit"
                    sh "mvn verify"
                    sh "cd .."
                }
            }
            stage("Build CraftBukkit") {
                withMaven(globalMavenSettingsConfig: "maven-dragonZone", mavenLocalRepo: "?/.m2/repository") {
                    sh "cd CraftBukkit"
                    sh "mvn verify"
                    sh "cd .."
                }
            }
            stage("Build Spigot") {
                withMaven(globalMavenSettingsConfig: "maven-dragonZone", mavenLocalRepo: "?/.m2/repository") {
                    sh "cd Spigot"
                    sh "mvn verify"
                    sh "cd .."
                }
            }
        }
    }

}
