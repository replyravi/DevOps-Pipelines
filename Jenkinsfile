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
        sh 'sed -i "" "s|WARN_LOGFILE.*|WARN_LOGFILE = warnings.log|" Doxyfile'  // macOS fix
        sh 'doxygen Doxyfile'
    }
}
    stage('Clone RepoC and Run Parser') {
    steps {
        git 'https://github.com/replyravi/RepoC.git'
        sh '''
            ls -la RepoC  # Verify RepoC contains parser.py and requirements.txt
            python3 -m venv venv
            source venv/bin/activate
            pip install --upgrade pip
            pip install -r RepoC/requirements.txt || echo "No dependencies found"
            python RepoC/parser.py warnings.log warnings.csv
            deactivate
        '''
    }
}


        stage('Archive Warnings CSV') {
            steps {
                archiveArtifacts artifacts: 'warnings.csv'
            }
        }
    }
}
