pipeline {
    agent any
    stages {
        stage('Fetch'){
            steps{
                sh 'echo "Yocto Fetch"'
                dir ('poky') {
                    git branch: 'sumo', url: 'http://git.yoctoproject.org/git/poky.git'
                    dir ('meta-raspberrypi') {
                        git branch: 'sumo', url: 'git://git.yoctoproject.org/meta-raspberrypi'
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
                               export MACHINE=\"raspberrpi3\" && \
                               source oe-init-build-env && \
                               sed -i 's/PACKAGE_CLASSES/#PACKAGE_CLASSES/g' ./conf/local.conf && \
                               bitbake-layers add-layer ../meta-raspberrypi && \
                               bitbake-layers add-layer ../meta-example && \
                               bitbake -c cleansstate minimal-example &&\
                               bitbake minimal-example" '''
                }
            }
        }            
    }
}
