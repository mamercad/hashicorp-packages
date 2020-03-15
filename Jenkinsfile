pipeline {
  agent any
  environment {
    PATH = '/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/sbin:/usr/local/bin'
  }
  parameters {
    string(name: 'version', defaultValue: '1.7.1', description: 'Consul version (https://www.consul.io/downloads.html)')
  }
  stages {
    stage('Build') {
      steps {
        sh("""
          sudo yum -y install rpm-build
          sudo yum -y install ruby ruby-devel
          ruby --version
          sudo gem install --no-document fpm
          which -a fpm
          fpm --version
          sudo yum -y install gnupg
          gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 51852D87348FFC4C # hashicorp
          curl -sLO https://releases.hashicorp.com/consul/${params.version}/consul_${params.version}_SHA256SUMS.sig
          curl -sLO https://releases.hashicorp.com/consul/${params.version}/consul_${params.version}_SHA256SUMS 
          gpg --verify consul_${params.version}_SHA256SUMS.sig consul_${params.version}_SHA256SUMS
          curl -sLO https://releases.hashicorp.com/consul/${params.version}/consul_${params.version}_linux_amd64.zip
          cat consul_${params.version}_SHA256SUMS
          grep consul_${params.version}_linux_amd64.zip consul_${params.version}_SHA256SUMS | tee consul_${params.version}_SHA256SUMS.tmp
          sha256sum -c consul_${params.version}_SHA256SUMS.tmp
          unzip -f consul_${params.version}_linux_amd64.zip
          fpm --input-type dir --output-type rpm --name consul --version ${params.version} --iteration ${BUILD_NUMBER} ./consul=/usr/local/bin/consul
        """)
      }
    }
  }
}
