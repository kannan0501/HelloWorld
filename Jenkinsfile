node { 
	checkout scm
	env.PATH ="${tool 'Maven3'}/bin:${env.PATH}"
	stash excludes: 'target/', includes: '**', name: 'source'
	stage('validate') {
		sh 'mvn validate'
	} 
	stage('compile') {
		sh 'mvn compile'
	} 
	stage('package') {
		 sh 'mvn clean package -DskipTests'
	}
	stage('install') {
		sh 'mvn clean install'
	} 
	stage('test') {
		parallel 'integration': {
			sh 'mvn clean verify'
		}, 'quality': {
			//sh 'mvn sonar:sonar'
			} 
	}
	stage('mailing') {
		
	}  
	stage('deploy') {
		unstash 'source'
		sh 'cp target/*.jar /opt/deploy'
	}

}
