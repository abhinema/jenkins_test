pipeline {
    agent any
    stages {
        stage('Fetch'){
            steps{
                sh 'echo "Yocto Fetch"'
                dir('fsl-imx6'){
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
               dir ('oe-core') {
                 sh '''bash -c "export BB_NUMBER_THREADS=64 && 
                        . export &&\
                        sed -i 's/PACKAGE_CLASSES/#PACKAGE_CLASSES/g' ./conf/local.conf && \
                        bitbake-layers add-layer ../meta-example && \
                        bitbake -k angstrom-qt-x11-image && \
                        bitbake angstrom-qt-x11-image -c populate_sdk" 
                        '''
                }
            }
        }            
    }
}
