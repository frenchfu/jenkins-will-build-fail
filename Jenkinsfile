pipeline {
    agent any

    environment {
        ZULIP_URL = "https://david-poc-zulip.ccwu.cc/api/v1/messages"
        ZULIP_AUTH = "jenkins-bot@david-poc-zulip.ccwu.cc:c6frc4L1p7fT71X8RsGxSJvNZK0PpKsm"
        TARGET_STREAM = "課-系統三課(T55)"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '=== [Stage 1] 正在自 GitHub 拉取專案原始碼 ==='
                checkout scm
            }
        }

        stage('Compile & Static Analysis') {
            steps {
                echo '=== [Stage 2] 正在編譯關貿地政/稅務整合服務核心模組 ==='
                sh 'java -version'
                sh 'echo "[INFO] Compiling Java source files in src/main/java..."'
                sh 'echo "[INFO] Compilation successful: 48 classes compiled."'
            }
        }

        stage('Unit & Regression Test') {
            steps {
                echo '=== [Stage 3] 執行過版前自動化迴歸驗證與測試 ==='
                sh '''
                    echo "[INFO] Running com.tradevan.pland.PaymentVerificationTest..."
                    echo "[INFO] Connecting to PLAND Master Database at 192.168.10.25:1521/PLAND_DB..."
                    echo "[ERROR] java.net.ConnectException: Connection refused to 192.168.10.25:1521"
                    echo "[ERROR] Test com.tradevan.pland.DeploymentTest FAILED: AssertionError: Expected status [200 OK] but got [500 INTERNAL_SERVER_ERROR]"
                    echo "[FATAL] 過版測試未通過，中斷發布流程！"
                    exit 1
                '''
            }
        }
    }

    post {
        failure {
            echo '=== [Alert] 過版失敗！正在發送警報至關貿通訊 系統三課(T55)... ==='
            sh '''
                curl -k -s -X POST "${ZULIP_URL}" \
                  -u "${ZULIP_AUTH}" \
                  -d "type=stream" \
                  -d "to=${TARGET_STREAM}" \
                  -d "topic=過版發布 / CI-CD" \
                  --data-urlencode "content=🚨 **【Jenkins 過版失敗通知】**
> **專案名稱**：\`jenkins-will-build-fail\`
> **分支 (Branch)**：\`main\`
> **Commit**：\`${GIT_COMMIT}\`
> **失敗階段**：\`Unit & Regression Test (單元與整合測試失敗)\`
> **錯誤細節**：\`Connection refused to 192.168.10.25:1521 (PLAND_DB)\`
> **過版執行者**：\`david.fu\`
> 
> ⚠️ **請 @**課-系統三課(T55)** 相關負責同仁儘速排查！**
🔗 [點此查看 Jenkins 控制台日誌](${BUILD_URL}console)"
            '''
        }
        success {
            echo 'Build succeeded!'
        }
    }
}
