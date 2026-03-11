node {

    checkout scm

    stage("Build") {
        docker.image('composer:2').inside('-u root') {
            sh '''
            cd src
            rm -f composer.lock
            composer install
            '''
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Pipeline Jenkins berhasil dijalankan"'
        }
    }

}
