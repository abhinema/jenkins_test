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
                dir('fsl-imx6'){
                   dir ('oe-core') {
                     sh '''bash -c "export BB_NUMBER_THREADS=64 && 
                            . export &&\
                            bitbake-layers add-layer ../meta-example && \
                            bitbake -k core-image-minimal && \
                            bitbake core-image-minimal -c populate_sdk" 
                            '''
                    }
                }
            }
        }            
    }
}
