@Library("jenkinslib") _
def tools = new org.devops.tool()

def MAVEN_HOME="/usr/local/maven/apache-maven-3.6.2/bin"
def branchName="*/master"
def srcUrl="https://github.com/YanXinYu1993/SpringBootDemo.git"
def selectProject="spring-boot-demo"
pipeline{
	agent { 
		node { label "build01"}
	}
	options {
		timestamps() //日志时间
		skipDefaultCheckout() // 删除隐式check out scm 语句
		disableConcurrentBuilds() //禁止并行
		timeout(time: 1, unit: 'HOURS') 
	}
	stages {
		stage ("Checkout"){
		    steps{
		        echo "开始检出"
		        sh "${MAVEN_HOME}/mvn -v"
		        checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]], 
										  doGenerateSubmoduleConfigurations: false, 
										  extensions: [], 
										  submoduleCfg: [], 
										  userRemoteConfigs: [[credentialsId: 'GitHub', url: "${srcUrl}"]]])
		    }
		}
		stage('Build') {
            steps {
                script{
                    echo "开始构建jar包"
                    echo "${selectProject}"
					tools.printMes("你好")
                    sh "${MAVEN_HOME}/mvn -f ${selectProject}/pom.xml clean package -Dmaven.test.skip=true"
                }
                
            }
        }
	
	}
	post {
		always {
			println("always")
		}
		success {
			script{
				currentBuild.description = "构建成功"
			}
		}
		failure{
			script{
				currentBuild.description = "构建失败"
			}
		}
		aborted{
			script{
				currentBuild.description = "构建取消"
			}
		}
	}
	

}
