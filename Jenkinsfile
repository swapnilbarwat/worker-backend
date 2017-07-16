def version = ''
node {
   stage('checkout') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/swapnilbarwat/worker-backend.git'
      version = readFile('version').trim()
      currentBuild.displayName = "${version}-${env.BRANCH_NAME}"
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
   }
   stage('Build') {
       docker.withRegistry('https://docker.io', 'docker-hub-credentials') {
          def app = docker.build("harshals/worker-backend:${version}")
          sh "docker push docker.io/harshals/worker-backend:${version}"
       }
       sh "curl -H \"Content-Type: application/x-yaml\" -X POST http://35.184.43.171:8080/api/v1/breeds --data-binary @breeds/worker.yml"
    }
}

node {
   stage('50-50% deployment') { // for display purposes
      input message: 'Deploy to cluster? This will rollout new build to 50% cluster.'
   }
}

node {
   stage('move full') { // for display purposes
      input message: 'Deploy to full cluster?'
   }
   stage('undeploy previous version') {
      echo "undeploying cluster"
   }
}