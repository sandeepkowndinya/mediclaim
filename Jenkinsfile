pipeline {
   agent any
	stages {
      stage('Git Checkout-mediclaim') {
         steps {
            git 'git@github.com:sandeepkowndinya/mediclaim.git'
		}
	}
	stage('Build with sonar') {
		steps {
			withSonarQubeEnv('sonar') {
				sh '/opt/maven/apache-maven-3.6.3/bin/mvn clean verify sonar:sonar -Dmaven.test.skip=true'
			}
		}
	}
	stage ('Deploy') {
		steps {
			sh '/opt/maven/apache-maven-3.6.3/bin/mvn clean deploy -Dmaven.test.skip=true'
		}
	}
	stage ('Release') {
		steps {
			sh 'export JENKINS_NODE_COOKIE=dontkillme ;nohup java -jar $WORKSPACE/target/*.jar &'
		}
	}
	stage ('DB Migration') {
		steps {
			sh '/opt/maven/apache-maven-3.6.3/bin/mvn clean flyway:migrate'
		}
	}
}
	//post {
        //always {
          //  emailext body: "${currentBuild.currentResult}: Project Name : ${env.JOB_NAME} Build ID : ${env.BUILD_NUMBER}\n\n Approval Link :  ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        //}
    //}

}
