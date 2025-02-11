pipeline {
    agent any
    environment {
        SHELL = "/bin/sh"
        PATH = "/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }
    stages {
        stage('Clone RepoA') {
            steps {
                script {
                    git credentialsId: 'github-cred', url: 'https://replyravi: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@github.com/replyravi/RepoA.git'
                }
            }
        }
        stage('Generate Doxygen Config') {
           steps {
                sh 'doxygen -g Doxyfile'
                sh 'sed -i "" "s|INPUT.*|INPUT = src|" Doxyfile'
                sh 'sed -i "" "s|GENERATE_HTML.*|GENERATE_HTML = YES|" Doxyfile'
                sh 'sed -i "" "s|GENERATE_LATEX.*|GENERATE_LATEX = NO|" Doxyfile'
            }
        }
        stage('Run Doxygen') {
            steps {
                sh '/bin/sh -c "doxygen Doxyfile"'
            }
        }
        stage('Archive Documentation') {
            steps {
                sh '/bin/sh -c "tar -czf doc.tar.gz html/"'
                archiveArtifacts artifacts: 'doc.tar.gz'
            }
        }
    }
}
