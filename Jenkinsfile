pipeline {
    agent { docker { image 'centos:7' } }
    stages {
        stage('build') {
            steps {
                sh('''
                  yum -y install ruby ruby-devel
                  ruby --version
                  #gem install --no-document fpm
                  #fpm --version
                  yum -y install gnupg
                  gpg --recv-keys 51852D87348FFC4C # hashicorp
                  #curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_SHA256SUMS.sig
                  #curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_SHA256SUMS 
                  #curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_linux_amd64.zip
                ''')
            }
        }
    }
}
