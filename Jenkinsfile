@Library('iti-sharedlib')_

properties([
    disableConcurrentBuilds()
])

node("java"){

    def javaHome = tool name: 'java-11', type: 'jdk'
    def mavenHome = tool name: 'mvn-3-5-4', type: 'maven'
    def DOCKER_USER = credentials('docker-username')
    def DOCKER_PASS = credentials('docker-password')

    stage("Get code"){
        checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Hassan-Eid-Hassan/java.git']])
    }
    stage("build app"){
        env.JAVA_HOME = javaHome
        env.PATH = "${javaHome}/bin:${mavenHome}/bin:${env.PATH}"
        sh 'java -version'
        sh 'mvn -version'
        def mavenBuild = new org.iti.mvn()
        mavenBuild.javaBuild("package install")
        hello("Ya 3m alooo rakezo m3ia")
    }
    stage("archive app"){
        archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
    }
    stage("docker build"){
        sh "docker build -t hassaneid/iti-java:v${BUILD_NUMBER} ."
        sh "docker images"
    }
}