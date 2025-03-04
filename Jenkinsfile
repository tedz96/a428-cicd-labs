node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000 -u root') {
        
        stage('Build') {
           sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh' 
        }
        stage ('Manual Approval'){
            input message: 'Lanjutkan ke tahap Deploy?'
        }
        stage('Deploy') {
            try {
                sh './jenkins/scripts/deliver.sh' 
                
                echo 'Menunggu selama 1 menit agar aplikasi bisa digunakan...'
                sleep time: 60, unit: 'SECONDS' 

                sh './jenkins/scripts/kill.sh' 
                echo 'Aplikasi berhasil dihentikan, pipeline selesai!'
            } catch (err) {
                echo "Error terjadi: ${err}"
                currentBuild.result = 'FAILURE'
            }
        }
    }
}
