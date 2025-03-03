node {
    def dockerImage = 'node:16-buster-slim'

    stage('Build') {
        docker.image(dockerImage).inside('--rm -p 3000:3000') {
            sh 'npm install'
        }
    }

    stage('Test') {
        docker.image(dockerImage).inside('--rm -p 3000:3000') {
            sh './jenkins/scripts/test.sh'
        }
    }

    stage('Deploy') {
        docker.image(dockerImage).inside('--rm -p 3000:3000') {
            try {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                sh './jenkins/scripts/kill.sh'
            } catch (err) {
                echo "Terjadi kesalahan saat deployment: ${err}"
                currentBuild.result = 'FAILURE'
            }
        }
    }
}
