pipeline{
    agent {
        label 'search-build'
    }

    
    parameters {
      choice(name: 'SEARCH_BRANCH', choices: ['development', 'test', 'stage', 'master', 'feature/SEARCH-2565-update-log4j-version-in-api-framework-and-bring-all-apis-to-use-same-framework', 'feature/SEARCH-2649-add-sockettimeout-search-api-framework'], description: 'Bit Bucket Branch')
      string(name: 'Release_Version', defaultValue: '', description: '', trim: true)
	  string(name: 'Development_Version', defaultValue: '', description: '', trim: true)
    }

    stages{
        stage('Checkout') {
            steps {
                git credentialsId: '6411ac46-4329-4b2e-afa4-824ebe7a4c48', url: 'ssh://git@bitbucket.fastenal.com:7999/sear/search-api-framework.git', branch: "${params.SEARCH_BRANCH}"
            }
        }

         stage('Maven Build Release') {
            steps {
                withMaven(maven: 'Maven 3.6.1'){
                        sh """mvn -B -f pom.xml -DdevelopmentVersion="${params.Development_Version}" -DreleaseVersion="${params.Release_Version}" versions:update-parent versions:update-properties -DallowDowngrade=false scm:checkin -Dmessage="POM Update" -DpushChanges -Dresume=false -DpreparationGoals="clean deploy" release:prepare release:perform -Darguments="-DaltDeploymentRepository=esearch_snapshots::default::https://nexus.fastenal.com/repository/esearch_snapshots/com/fastenal/esearch/search-api-framework/" """     
                 
                }
            }
        }
    }

    post {
            always {
                cleanWs()
            }
        }
}




pipeline{
    agent{
        label 'search-build'

    }
    tools {
        maven 'Maven 3.6.1' // Specify the Maven version here
    }
    
   
    parameters {
        choice(
            name: 'SEARCH_BRANCH',
            choices: [ 'test', 'stage', 'master', 'SEARCH-2852-elastic-cloud-initial-cost-analysis-using-product-index-and-api', 'feature/SEARCH-2934-update-search-indexer-framework-for-elastic-cloud', 'feature/SEARCH-2934-2-update-search-indexer-framework-for-elastic-cloud'],
            description: 'Java Search Indexer Framework'
        )
        string(name: 'Release_Version', defaultValue: '', description: '', trim: true)
		string(name: 'Development_Version', defaultValue: '', description: '', trim: true)
    }

    stages {
         
        stage('Checkout') {
            steps {
                git credentialsId: '6411ac46-4329-4b2e-afa4-824ebe7a4c48', url: 'ssh://git@bitbucket.fastenal.com:7999/sear/search-indexer-framework.git', branch: "${params.SEARCH_BRANCH}"
            }
        }
        stage('Maven Build Release') {
            steps {
                withMaven(maven: 'Maven 3.6.1'){
                    
                    sh """mvn -B -f pom.xml -DdevelopmentVersion="${params.Development_Version}" -DreleaseVersion="${params.Release_Version}" versions:update-parent versions:update-properties -DallowDowngrade=false scm:checkin -Dmessage="POM Update" -DpushChanges -Dresume=false -DpreparationGoals="clean deploy" release:prepare release:perform -Darguments="-DaltDeploymentRepository=esearch_snapshots::default::https://nexus.fastenal.com/repository/esearch_snapshots/com/fastenal/esearch/search-indexer-framework/" """
                }
            }
        }
      
    }
   
    post {
        always {
            cleanWs()
        }
    }
    
}
