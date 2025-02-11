pipeline {
    agent any
    stages {
        stage('Clone RepoA') {
            steps {
                git 'https://github.com/replyravi/RepoA.git'
            }
        }
        stage('Generate Doxygen Config') {
            steps {
                sh 'doxygen -g Doxyfile'
                sh 'sed -i "s|INPUT.*|INPUT = src|" Doxyfile'
                sh 'sed -i "s|GENERATE_HTML.*|GENERATE_HTML = YES|" Doxyfile'
                sh 'sed -i "s|GENERATE_LATEX.*|GENERATE_LATEX = NO|" Doxyfile'
            }
        }
        stage('Run Doxygen') {
            steps {
                sh 'doxygen Doxyfile'
            }
        }
        stage('Archive Documentation') {
            steps {
                sh 'tar -czf doc.tar.gz html/'
                archiveArtifacts artifacts: 'doc.tar.gz'
            }
        }
    }
}
