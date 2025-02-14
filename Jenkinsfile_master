pipeline {
  agent any
  tools	{
	nodejs	'nodejs-23.4'
  }
  
  stages {
    stage('VM Node Version') {
      steps {
       sh '''
          node -v
          npm -v

       '''
       }

    }
  }
}

