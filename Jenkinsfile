pipeline {
    agent any
    stages {
        stage('build project1 microservice') {
            when {
                changeset "**/microservices/project1/*.*"
            }
            steps {
                echo 'Executing build of project1 microservice'
            }
        }
        stage('build project2 microservice') {
            when {
                changeset "**/microservices/project2/*.*"
            }
            steps {
                echo 'Executing build of project2 microservice'
            }
        }
        stage('build project1 microfrontend') {
            when {
                changeset "**/microfrontends/project1/*.*"
            }
            steps {
                echo 'Executing build of project1 microfrontend'
            }
        }
        stage('build project2 microfrontend') {
            when {
                changeset "**/microfrontends/project2/*.*"
            }
            steps {
                echo 'Executing build of project2 microfrontend'
            }
        }
    }
}