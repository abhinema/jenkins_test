pipeline {
    agent any
    stages {
        stage('Fetch'){
            steps{
                sh 'echo "Yocto Fetch"'
                dir ('poky') {
                    git branch: 'sumo', url: 'http://git.yoctoproject.org/git/poky.git'
                    dir ('oe-core') {
                        sh ''' repo init -u http://git.toradex.com/toradex-bsp-platform.git -b LinuxImageV2.7
                               repo sync '''
                    }
                    dir ('meta-example') {
                        git branch: 'master', url: 'https://github.com/abhinema/meta-example.git'
                    }
                }
            }
        }
        stage('Build') {
            steps {
               dir ('poky') {
                sh '''bash -c "export BB_NUMBER_THREADS=64 && 
                               export MACHINE=\"colibri-t30\" && \
                               source oe-init-build-env && \
                               sed -i 's/PACKAGE_CLASSES/#PACKAGE_CLASSES/g' ./conf/local.conf && \
                               bitbake minimal-example" '''
                }
            }
        }            
    }
}
