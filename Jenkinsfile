pipeline {
    agent { docker { image 'ruby' } }
    stages {
        stage('build') {
            steps {
                sh('''
                  ruby --version
                  gem install --no-document fpm
                  fpm --version
                ''')
            }
        }
    }
}
