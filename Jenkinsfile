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

// node {

//     stage('Install Dependency') {
//         dir('src') {
//             sh 'composer install'
//         }
//     }

//     stage('Prepare Laravel') {
//         dir('src') {
//             sh 'cp .env.example .env || true'
//             sh 'php artisan key:generate || true'
//         }
//     }

// }

node {

    stage('Checkout Code') {
        checkout scm
    }

    stage('Install Dependency') {
        dir('src') {
            sh 'composer install --no-interaction --prefer-dist --no-scripts'
        }
    }

    stage('Prepare Laravel') {
        dir('src') {
            sh 'cp .env.example .env || true'
            sh 'php artisan key:generate || true'
        }
    }

    stage('Laravel Check') {
        dir('src') {
            sh 'php artisan --version'
        }
    }

    stage('Deploy to Production') {
        dir('src') {
            docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
                sshagent(credentials: ['ssh-prod']) {
                    sh 'mkdir -p ~/.ssh'
                    sh 'ssh-keyscan -H "$PROD_HOST" > ~/.ssh/known_hosts'
                    sh '''
                    rsync -rav --delete ./ \
                    ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
                    --exclude=.env \
                    --exclude=storage \
                    --exclude=.git
                    '''
                }
            }
        }
    }

}