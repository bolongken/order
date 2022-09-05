pipeline{
	agent{
  		label 'jenkins_node_47.94.155.188'
	}

	stages {

	  stage('pull code'){
	  	steps{
	  		checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'order']], userRemoteConfigs: [[credentialsId: '47b4c211-2575-4c09-ba44-61e61a1e2a24', url: 'git@github.com:bolongken/order.git']]])
	  	}
	  }
	  stage('执行脚本') {
	    steps {
	      sh '''#jenkins如果在shell里使用nohup发现还是不能后台运行，直接挂掉。
				#那么可以在jenkins命令里加上BUILD_ID=dontKillMe解决
				BUILD_ID=DONTKILLME

				source /etc/profile
				#配置运行参数
				export PROJ_PATH=`pwd`
				export TOMCAT_APP_PATH=/usr/local/soft/tomcat

				sh $PROJ_PATH/order/deploy.sh'''
	    }
	  }
	  stage('publish'){
	  	steps{
	  		sshPublisher(publishers: [sshPublisherDesc(configName: 'SSH_Server_47.94.155.188', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cp -a /usr/local/soft/tomcat/webapps/ROOT.war /home/temp/ROOT_simple.war', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '/usr/local/soft/tomcat/webapps/ROOT.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	  	}
	  }
	}
}