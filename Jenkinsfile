pipeline {
    agent any
    stages {
        stage('Fetch'){
            steps{
                sh 'echo "Yocto Fetch"'
                dir('fsl-imx6'){
                    dir ('oe-core') {
                        sh ''' python --version && \
                               openssl version -a
                           '''  
                        sh ''' repo init -u http://git.toradex.com/toradex-bsp-platform.git -b LinuxImageV2.7
                               repo sync '''
                    }
                    dir ('meta-example') {
                        git branch: 'fsl-imx6', url: 'https://github.com/abhinema/meta-example.git'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                dir('fsl-imx6'){
                   dir ('oe-core') {
                     sh ''' bash -c "export BB_NUMBER_THREADS=64 && \
                            export MACHINE=apalis-imx6 && \
                            . export &&\
                            bitbake -c cleansstate angstrom-lxde-image && \
                            bitbake -k angstrom-lxde-image " 
                        '''
                    }
                }
            }
        }            
    }
}
