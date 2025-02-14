pipeline {
  agent any
  tools	{
	nodejs	'nodejs-23.4'
  }
  
  stages {
    stage('Installing Dependencies') {
      steps {
       sh 'npm install --no-audit'
       }

    }
     stage('NPM Dependency Audits') {
      steps {
       sh '''
		npm audit --audit-level=critical
		echo $?	
	'''	
       }

    }
  }
}

