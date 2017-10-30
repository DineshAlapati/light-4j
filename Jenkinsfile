#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        skipDefaultCheckout()
        timeout(time: 1, unit: 'HOURS')
    }
    tools {
        maven 'MAVEN3'
    }
    stages {
        stage('Clean Workspace') {
            steps {  
                // Wipe the workspace so we are building completely clean
                deleteDir()
            }
        }
        stage('Build light-4j') {
            steps {
                dir('light-4j') {
                    // Git clone. Get code from GitHub repository
                    git([url: 'https://github.com/DineshAlapati/light-4j.git', branch: 'develop'])
                    // Run the maven build
                    sh 'mvn -Dmaven.test.failure.ignore clean package'
                }
            }
        }
        stage('Build light-rest-4j') {
            steps {
                dir('light-rest-4j') {
                    // Git clone. Get code from GitHub repository
                    git([url: 'https://github.com/DineshAlapati/light-rest-4j.git', branch: 'develop'])
                    // Run the maven build
                    sh 'mvn -Dmaven.test.failure.ignore clean package'
                }
            }
        }
        stage('Results') {
            steps {
                dir('light-4j') {
                    echo 'Build Results..'
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archive 'target/*.jar'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}