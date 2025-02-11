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
                    git credentialsId: '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', url: 'https://github.com/your-github-username/RepoA.git'
                }
            }
        }
        stage('Generate Doxygen Config') {
            steps {
                sh '/bin/sh -c "doxygen -g Doxyfile"'
                sh '/bin/sh -c "sed -i \"s|INPUT.*|INPUT = src|\" Doxyfile"'
                sh '/bin/sh -c "sed -i \"s|GENERATE_HTML.*|GENERATE_HTML = YES|\" Doxyfile"'
                sh '/bin/sh -c "sed -i \"s|GENERATE_LATEX.*|GENERATE_LATEX = NO|\" Doxyfile"'
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
