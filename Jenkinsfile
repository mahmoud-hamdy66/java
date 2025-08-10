properties([
    disableConcurrentBuilds(),
    parameters([
        string(defaultValue: '${BUILD_NUMBER}', name: 'XYZ'),
        string(defaultValue: "tool name: 'java-11', type: 'jdk'", name: 'javaHome')
    ])
])

node("java"){

    def mavenHome = tool name: 'mvn-3-5-4', type: 'maven'
    def DOCKER_USER = credentials('docker-username')
    def DOCKER_PASS = credentials('docker-password')

    stage("Get code"){
        checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Hassan-Eid-Hassan/java.git']])
    }
    stage("build app"){
        echo '$XYZ'
        echo "$XYZ"
        echo "\$XYZ"
        echo '\$XYZ'
        env.JAVA_HOME = javaHome
        env.PATH = "${javaHome}/bin:${mavenHome}/bin:${env.PATH}"
        sh 'java -version'
        sh 'mvn -version'
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