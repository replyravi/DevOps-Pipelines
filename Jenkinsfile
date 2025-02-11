pipeline {
    agent any
environment {
        SHELL = "/bin/sh"
        PATH = "/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }
    stages {
        stage('Clone RepoA') {
            steps {
                git credentialsId: 'github-cred', url: 'https://replyravi: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@github.com/replyravi/RepoA.git'
            }
        }
        stage('Generate Doxygen Warnings') {
            steps {
                sh 'doxygen -g Doxyfile'
                sh 'sed -i "s|WARN_LOGFILE.*|WARN_LOGFILE = warnings.log|" Doxyfile'
                sh 'doxygen Doxyfile'
            }
        }
        stage('Clone RepoC and Run Parser') {
            steps {
                git 'https://github.com/replyravi/RepoC.git'
                sh 'pip install -r RepoC/requirements.txt'
                sh 'python RepoC/parser.py warnings.log warnings.csv'
            }
        }
        stage('Archive Warnings CSV') {
            steps {
                archiveArtifacts artifacts: 'warnings.csv'
            }
        }
    }
}
