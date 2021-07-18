pipeline {
    agent any
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git url: 'https://github.com/Tiru456/samplejavaapp.git'
		sh script: '/usr/share/apache-maven/mvn compile'
           }
        }
        stage('codereview-pmd') {
	   steps {
                echo 'codereview..'
		sh script: '/usr/share/apache-maven/mvn -P metrics pmd:pmd'
           }
	   post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        sh script: '/usr/share/apache-maven/mvn test'
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
        stage('codecoverate') {
	   steps {
                echo 'codecoverage..'
		sh script: '/usr/share/apache-maven/mvn cobertura:cobertura -Dcobertura.report.format=xml'
           }
	   post {
               success {
	           cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false                  
               }
           }		
        }
        stage('package') {
	   steps {
                echo 'package..'
		sh script: '/usr/share/apache-maven/mvn package'	
           }		
        }
    }
}
