pipeline {
  agent any
  tools	{
	nodejs	'nodejs-23.4'
  }
  
  stages {
    stage('VM Node Version') {
      step {
       sh '''
          node -v
          npm -v

       '''
       }

    }
  }
}

