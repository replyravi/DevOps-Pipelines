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
        sh '''
            rm -rf RepoC  # Ensure clean state
            git clone https://github.com/replyravi/RepoC.git
            ls -la RepoC  # Verify RepoC exists
            cd RepoC
            python3 -m venv venv
            source venv/bin/activate
            pip install --upgrade pip
            pip install -r requirements.txt || echo "No dependencies found"
            python parser.py ../warnings.log ../warnings.csv
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
