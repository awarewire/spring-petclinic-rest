pipeline {
    agent {
        docker {
            image 'maven:3.8.8-eclipse-temurin-17-alpine'
        }
    }
    // tools {
    //     maven 'maven3.8.8'
    // }
    triggers {
        // cron('* * * * *')
        githubPush()
    }
    stages {
        // stage('Checkout SCM') {
        //     steps {
        //         git branch: 'master', url: 'https://github.com/devops-mitocode/spring-petclinic-rest.git'
        //     }
        // }
        // stage('Compile') {
        //     steps {
        //         sh 'mvn clean compile -B -ntp'
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         sh 'mvn test -B -ntp'
        //     }
        //     post { 
        //         success { 
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //         failure { 
        //             echo 'Tests failed!'
        //         }
        //     }  
        // }
        // stage('Coverage') {
        //     steps {
        //         sh 'mvn jacoco:report -B -ntp'
        //     }
        //     post { 
        //         success { 
        //             // recordCoverage(tools: [[parser: 'JACOCO']])


        //             discoverReferenceBuild()
        //             recordCoverage(
        //                 tools: [[parser: 'JACOCO']],
        //                 sourceCodeRetention: 'EVERY_BUILD',
        //                 qualityGates: [
        //                     [threshold: 80.0, metric: 'LINE', criticality: 'FAILURE'],
        //                     [threshold: 60.0, metric: 'BRANCH', criticality: 'FAILURE']
        //                 ]
        //             )
        //         }
        //     }  
        // }
        stage('Package') {
            steps {
                sh 'env | sort'
                sh 'mvn clean package -DskipTests -B -ntp'
            }
        }
        // stage('SonarQube') {
        //     steps {
        //         withSonarQubeEnv('sonarqube') {
        //             sh 'env | sort'
        //             script {
        //                 def branchName = GIT_BRANCH.replaceFirst('^origin/', '')
        //                 println "Branch name: ${branchName}"
        //                 sh "mvn sonar:sonar -B -ntp -Dsonar.branch.name=${branchName}"
        //             }
        //         }
        //     }
        // }
        // stage('SonarQube Quality Gate') {
        //     steps {
        //         timeout(time: 1, unit: 'MINUTES') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }
        // stage('SonarQube') {
        //     steps {
        //         withSonarQubeEnv('sonarqube'){
        //             sh 'env | sort'
        //             script {
        //                 if (env.CHANGE_ID) {
        //                     sh """
        //                         mvn sonar:sonar -B -ntp \
        //                         -Dsonar.pullrequest.key=${env.CHANGE_ID} \
        //                         -Dsonar.pullrequest.branch=${env.CHANGE_BRANCH} \
        //                         -Dsonar.pullrequest.base=${env.CHANGE_TARGET}
        //                     """
        //                 } else {
        //                     def branchName = GIT_BRANCH.replaceFirst('^origin/', '')
        //                     println "Branch name: ${branchName}"
        //                     sh "mvn sonar:sonar -B -ntp -Dsonar.branch.name=${branchName}"
        //                 }
        //             }
        //         }
        //     }
        // }
        stage('Artifactory') {
            steps {
                script {
                    // Forma 1: Usando rtMaven
                    // sh 'env | sort'
                    // env.MAVEN_HOME = '/usr/share/maven'

                    // def snapshot = 'spring-petclinic-rest-snapshot'
                    // def release = 'spring-petclinic-rest-release'

                    // def server = Artifactory.server 'artifactory'
                    // def rtMaven = Artifactory.newMavenBuild()
                    // rtMaven.deployer server: server, snapshotRepo: snapshot, releaseRepo: release
                    // def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install -DskipTests -B -ntp'

                    // server.publishBuildInfo buildInfo

                    // Forma 2: FileSpec
                    def server = Artifactory.server 'artifactory'
                    def targetRepo = 'spring-petclinic-rest-release'
                    
                    def pom = readMavenPom file: 'pom.xml'
                    println pom.groupId
                    def groupIdPath = pom.groupId.replaceAll("\\.", "/")
                    println groupIdPath

                    def uploadSpec = """
                        {
                            "files": [
                                {
                                    "pattern": "target/.*.jar",
                                    "target": "${targetRepo}/${groupIdPath}.${pom.artifactId}.${pom.version}/",
                                    "regexp": "true",
                                    "props": "build.url=${RUN_DISPLAY_URL};build.user=${USER}"
                                }
                            ]
                        }
                    """
                    server.upload spec: uploadSpec
                }
            }
        }        
    }
    post { 
        success { 
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            cleanWs()
        }
    }    
}