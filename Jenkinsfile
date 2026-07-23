// Git仓库地址
def git_url = "git@github.com:17755331737/back.git"

// 分支名称
def branch = "master"

// 凭证ID（Jenkins中配置的SSH凭证ID）
def auth_id = "git"

//镜像的版本号
def tag = "latest"

//harbor的url地址
def harbor_url = "192.168.42.129:85"

//harbor登录凭证
def harbor_auth = "harbor"

//镜像仓库名称
def harbor_project = "test"


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
stage('删除旧镜像') {
    sh "docker rmi -f  ${project}:${tag} || true"
    sh "docker rmi -f ${harbor_url}/${harbor_project}/${project}:${tag} || true"
    sh " docker image prune -f"
        }


stage('编译打包子工程') {
    sh "mvn -f ${project} clean install dockerfile:build"
    //定义镜像名称
    def imagename = "${project}:${tag}"

    //对镜像打标签
    sh "docker tag ${imagename} ${harbor_url}/${harbor_project}/${imagename}"

    //推送镜像到harbor仓库
    withCredentials([usernamePassword(credentialsId: "${harbor_auth}", passwordVariable: 'password', usernameVariable: 'username')]) {
    // some block

    //登录harbor
    sh "docker login  -u ${username} -p ${password} ${harbor_url}"

    //镜像上传
    sh "docker push  ${harbor_url}/${harbor_project}/${imagename}"
    sh "echo '镜像推送完成'"
    
}

	}
}

