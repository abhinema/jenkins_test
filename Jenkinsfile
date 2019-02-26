pipeline {
    agent any
    stages {
        stage('Fetch'){
            steps{
                sh 'echo "Yocto Fetch"'
                dir ('poky') {
                    git branch: 'sumo', url: 'http://git.yoctoproject.org/git/poky.git'
                    dir ('meta-example') {
                        git branch: 'master', url: 'https://github.com/abhinema/meta-example.git'
                    }
                }
            }
        }
        stage('Build') {
            steps {
               dir ('poky') {
                sh 'bash - bitbake -c cleansstate cpcmd'   
                sh '''bash -c "export BB_NUMBER_THREADS=64 && source oe-init-build-env && \
                        bitbake-layers add-layer ../meta-example && \
                        bitbake core-image-minimal" '''
                }
            }
        }            
    }
}
