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
                sh 'env | sort'
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