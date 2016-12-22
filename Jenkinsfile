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
            withMaven(globalMavenSettingsConfig: "maven-dragonZone", mavenLocalRepo: "?/.m2/repository") {
                stage("Build Bukkit") {
                    dir("Bukkit") {
                        sh "mvn verify"
                    }
                }
                stage("Build CraftBukkit") {
                    dir("CraftBukkit") {
                        sh "mvn verify"
                    }
                }
                stage("Build Spigot") {
                    dir("Spigot") {
                        sh "mvn verify"
                    }
                }
            }
        }
    }
}
