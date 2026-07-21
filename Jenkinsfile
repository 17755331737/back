pipeline {
    agent any
    
    parameters {
        string name: 'branch', defaultValue: 'master', description: '请输入要构建的分支'
    }
    
    options {
        timeout(time: 30, unit: 'MINUTES')  // 防止卡死
        skipDefaultCheckout()               // 手动控制 checkout
    }
    
    stages {
        stage('拉取代码') {
            steps {
                checkout scmGit(
                    branches: [[name: "*/${params.branch}"]],
                    extensions: [
                        [$class: 'CloneOption', timeout: 120, shallow: false],
                        [$class: 'CleanBeforeCheckout']
                    ],
                    userRemoteConfigs: [[
                        credentialsId: 'git',        // 确保和你的 Credentials ID 一致
                        url: 'git@github.com:17755331737/back.git'
                    ]]
                )
            }
        }
        
        // 后面可以继续添加 build、test、deploy 等 stage
        stage('构建') {
            steps {
                echo "当前构建分支: ${params.branch}"
                sh 'ls -la'   // 测试 workspace 是否生成
            }
        }
    }
}
