#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    gitlabCommitStatus('build') {
        docker.image('jhipster/jhipster:v6.0.1').inside('-u jhipster -e MAVEN_OPTS="-Duser.home=./"') {
            stage('check java') {
                sh "java -version"
            }

            stage('clean') {
                sh "chmod +x mvnw"
                sh "./mvnw clean"
            }

            stage('backend tests') {
                try {
                    sh "./mvnw verify"
                } catch(err) {
                    throw err
                } finally {
                    junit '**/target/test-results/**/TEST-*.xml'
                }
            }

            stage('packaging') {
                sh "./mvnw verify -Pprod -DskipTests"
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}
