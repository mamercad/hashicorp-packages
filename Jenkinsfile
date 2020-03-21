pipeline {
  agent any
  triggers {
    GenericTrigger(
      genericVariables: [
        [key: 'ref', value: '$.ref']
      ],
      causeString: 'Triggered on $ref',
      token: 'hunter2',
      printContributedVariables: true,
      printPostContent: true,
      silentResponse: false,
      regexpFilterText: '$ref',
      regexpFilterExpression: 'refs/heads/' + BRANCH_NAME
    )
  }
  environment {
    PATH = '/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/sbin:/usr/local/bin'
  }
  parameters {
    string(name: 'consul_version', defaultValue: '1.7.1', description: 'Consul version (https://www.consul.io/downloads.html)')
    string(name: 'vault_version', defaultValue: '1.3.4', description: 'Vault version (https://www.vaultproject.io/downloads/)')
  }
  stages {
    stage('Build Consul') {
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
          curl -sLO https://releases.hashicorp.com/consul/${params.consul_version}/consul_${params.consul_version}_SHA256SUMS.sig
          curl -sLO https://releases.hashicorp.com/consul/${params.consul_version}/consul_${params.consul_version}_SHA256SUMS 
          gpg --verify consul_${params.consul_version}_SHA256SUMS.sig consul_${params.consul_version}_SHA256SUMS
          curl -sLO https://releases.hashicorp.com/consul/${params.consul_version}/consul_${params.consul_version}_linux_amd64.zip
          cat consul_${params.consul_version}_SHA256SUMS
          grep consul_${params.consul_version}_linux_amd64.zip consul_${params.consul_version}_SHA256SUMS | tee consul_${params.consul_version}_SHA256SUMS.tmp
          sha256sum -c consul_${params.consul_version}_SHA256SUMS.tmp
          unzip -o consul_${params.consul_version}_linux_amd64.zip
          fpm --input-type dir --output-type rpm --name consul --version ${params.consul_version} --iteration ${BUILD_NUMBER} ./consul=/usr/local/bin/consul
        """)
      }
    }
    stage('Build Vault') {
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
          curl -sLO https://releases.hashicorp.com/vault/${params.vault_version}/vault_${params.vault_version}_SHA256SUMS.sig
          curl -sLO https://releases.hashicorp.com/vault/${params.vault_version}/vault_${params.vault_version}_SHA256SUMS 
          gpg --verify vault_${params.vault_version}_SHA256SUMS.sig vault_${params.vault_version}_SHA256SUMS
          curl -sLO https://releases.hashicorp.com/vault/${params.vault_version}/vault_${params.vault_version}_linux_amd64.zip
          cat vault_${params.vault_version}_SHA256SUMS
          grep vault_${params.vault_version}_linux_amd64.zip vault_${params.vault_version}_SHA256SUMS | tee vault_${params.vault_version}_SHA256SUMS.tmp
          sha256sum -c vault_${params.vault_version}_SHA256SUMS.tmp
          unzip -o vault_${params.vault_version}_linux_amd64.zip
          fpm --input-type dir --output-type rpm --name vault --version ${params.vault_version} --iteration ${BUILD_NUMBER} ./vault=/usr/local/bin/vault
        """)
      }
    }
  }
}
