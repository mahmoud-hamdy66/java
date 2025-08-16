@Library('iti-sharedlib')_

properties([
    disableConcurrentBuilds()
])

node {

    def javaHome = tool name: 'java-11', type: 'jdk'
    def mavenHome = tool name: 'mvn-3-5-4', type: 'maven'

    stage("Get code"){
        checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Hassan-Eid-Hassan/java.git']])
    }

    stage("Build app"){
        env.JAVA_HOME = javaHome
        env.PATH = "${javaHome}/bin:${mavenHome}/bin:${env.PATH}"
        sh 'java -version'
        sh 'mvn -version'

        def mavenBuild = new org.iti.mvn()
        mavenBuild.javaBuild("package install")
    }

    stage("Archive app"){
        archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
    }

    stage("Docker build"){
        def docker = new com.iti.docker()
        docker.build("iti-java", "${BUILD_NUMBER}")
    }

    stage("Push Java app image"){
        // استخدم withCredentials عشان Docker login يشتغل صح
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            def docker = new com.iti.docker()
            docker.login(DOCKER_USER, DOCKER_PASS)
            docker.push("iti-java", "${BUILD_NUMBER}")
        }
    }

    stage("Update ArgoCD deployment"){
        sh "mkdir -p argocd"
        dir("argocd") {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Hassan-Eid-Hassan/argocd.git']])
            sh "sed -i 's#        image: .*#        image: iti-java:${BUILD_NUMBER}#' iti-dev/deployment.yaml"
        }
    }
}
