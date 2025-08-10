node("java"){
    tool name: 'java-11', type: 'jdk'
    tool name: 'mvn-3-5-4', type: 'maven'
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