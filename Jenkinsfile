// node {

//     checkout scm

//     stage("Build") {
//         docker.image('composer:2').inside('-u root') {
//             sh '''
//             cd src
//             rm -f composer.lock
//             composer install
//             '''
//         }
//     }

//     stage("Testing") {
//         docker.image('ubuntu').inside('-u root') {
//             sh 'echo "Pipeline Jenkins berhasil dijalankan"'
//         }
//     }

// }

node {
    stage('Install Dependency') {
        sh 'composer install'
    }

    stage('Prepare Laravel') {
        sh 'cp .env.example .env || true'
        sh 'php artisan key:generate || true'
    }
}