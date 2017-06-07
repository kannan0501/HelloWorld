node { 
	checkout scm
	env.PATH ="${tool 'Maven3'}/bin:${env.PATH}"
	stash excludes: 'target/', includes: '**', name: 'source'
	emailext attachLog: true,body: 'Test', compressLog: true, subject: 'Test jenkins Pipelines', to: 'sgandra@altimetrik.com,snachiappan@altimetrik.com' 
	properties([pipelineTriggers([cron('0 10 * * *')])])
	checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/kannan0501/HelloWorld.git']]]) 
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
			//sh 'gradle sonar:sonar'
			} 
	} 
	stage('deploy') {
		unstash 'source'
		sh 'cp target/*.jar /opt/deploy'
	}
}
