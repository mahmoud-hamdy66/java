node("java"){
    tools{
        maven 'mvn-3-5-4'
        jdk 'java-11'
    }
    environment{
        DOCKER_USER = credentials('docker-username')
        DOCKER_PASS = credentials('docker-password')
    }
    parameters {
        string defaultValue: '${BUILD_NUMBER}', name: 'XYZ'
    }
    stage("build app"){
        sh "mvn package install"
    }
    stage("archive app"){
        archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
    }
    stage("docker build"){
        sh "docker build -t hassaneid/iti-java:v${XYZ} ."
        sh "docker images"
    }
}