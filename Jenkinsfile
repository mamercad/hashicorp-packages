pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh('''
                  PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/sbin:/usr/local/bin
                  sudo yum -y install ruby ruby-devel
                  ruby --version
                  sudo gem install --no-document fpm
                  which -a fpm
                  fpm --version
                  sudo yum -y install gnupg
                  gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 51852D87348FFC4C # hashicorp
                  curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_SHA256SUMS.sig
                  curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_SHA256SUMS 
                  gpg --verify consul_1.7.1_SHA256SUMS.sig consul_1.7.1_SHA256SUMS
                  curl -sLO https://releases.hashicorp.com/consul/1.7.1/consul_1.7.1_linux_amd64.zip
                  grep consul_1.7.1_linux_amd64.zip consul_1.7.1_SHA256SUMS | tee consul_1.7.1_SHA256SUMS
                  sha256sum -c consul_1.7.1_SHA256SUMS
                ''')
            }
        }
    }
}
