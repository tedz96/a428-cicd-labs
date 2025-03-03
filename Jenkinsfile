node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000 -u root') {
        
        stage('Build') {
            echo 'update2minutes'
            sh 'npm install'
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh' 
        }
        stage('Deploy') {
        try {
            sh './jenkins/scripts/deliver.sh'
            def userInput = input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
            sh './jenkins/scripts/kill.sh'
        } catch (err) {
            echo "Deployment gagal: ${err}"
            } 
        }
    }
}   