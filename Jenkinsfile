// Git仓库地址
def git_url = "git@github.com:17755331737/back.git"

// 分支名称
def branch = "master"

// 凭证ID（Jenkins中配置的SSH凭证ID）
def auth_id = "git"

node {
    stage('拉取代码') {
        checkout scmGit(
            branches: [[name: "*/${branch}"]],
            extensions: [],
            userRemoteConfigs: [[
                credentialsId: auth_id,
                url: git_url
            ]]
        )
    }
}
