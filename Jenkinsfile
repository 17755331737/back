// Git仓库地址
def git_url = "git@github.com:17755331737/back.git"

// 分支名称
def branch = "master"

// 凭证ID（Jenkins中配置的SSH凭证ID）
def auth_id = "git"

node {
stage('拉取代码') {
    checkout scmGit(branches: [[name: "*/${branch}"]], extensions: [], userRemoteConfigs: [[credentialsId: "${auth_id}", url: "${git_url}"]])
                  }
stage('代码审查') {
    //定义当前jenkins的sonarqube-scanner工具，工具名称可在系全局工具配置里面找到
	def scannerHome = tool 'sonarqube-scanner'
	//引用当前Jenkinssonar环境，可在系统设置的环境变量里面找到
	withSonarQubeEnv('sonarqube'){
				sh"""
					cd ${project}
					${scannerHome}/bin/sonar-scanner
				"""
			}	
}
stage('编译打包子工程') {
    sh "mvn -f ${project} clean install"
	}
}
