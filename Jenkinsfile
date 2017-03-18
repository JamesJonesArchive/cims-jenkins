node('jenkins') {
  checkout scm
  
  stage('Install Ansible') {
    def distVer = sh script: 'python -c "import platform;print(platform.linux_distribution()[1])"', returnStdout: true
    def missingEpel = sh script: 'rpm -q --quiet epel-release', returnStatus: true
    def missingAnsible = sh script: 'rpm -q --quiet ansible', returnStatus: true
    if (missingEpel) {
      echo "Epel to be installed"
      if(distVer.toFloat().trunc() == 7) {
        echo "Detected version 7"
        sh "rpm -iUvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm || exit 0"
      }
      if(distVer.toFloat().trunc() == 6) {
        echo "Detected version 6"
        sh "rpm -iUvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm || exit 0"
      }
      if(distVer.toFloat().trunc() == 5) {
        echo "Detected version 5"
        sh "rpm -iUvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-5.noarch.rpm || exit 0"
      }
      sh "yum -y update || exit 0"
    } else {
      echo "Already installed"
    }
    if(missingAnsible) {
      sh "yum -y install ansible || exit 0"
    }
    // sh 'yum -y install rpms/ansible-vault-usf*.rpm || exit 0'
    unstash 'ansible'
  }
}