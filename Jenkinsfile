pipeline {
    agent any

    environment {
      MAVEN_ARGS = "-e clean install"
      DOCKER_IMAGE = 'knightboyz/springboot-app:latest'
      KUBE_CONFIG_PATH = '/Users/anouza/.kube/config'    // เปลี่ยนตามตำแหน่งที่เก็บ kubeconfig บน Jenkins
      KUBE_NAMESPACE = 'default'                              // ชื่อ namespace ที่ใช้ใน Kubernetes
    }

    stages {
        stage('Build Maven Project') {
            steps {
                // รัน Maven เพื่อ build project
                withMaven(maven: 'MAVEN_HOME') {
                    sh "mvn ${MAVEN_ARGS}"
                }
            }
        }

        // stage('Run Spring Application'){
        //     steps {
        //         // withEnv(["PATH=/usr/local/bin:$PATH;JENKINS_NODE_COOKIE=do_not_kill"]){
        //         withEnv(["JENKINS_NODE_COOKIE=do_not_kill"]){
        //             sh "nohup java -jar target/demo-0.0.1-SNAPSHOT.jar &"
        //         }
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                withEnv(["PATH=/usr/local/bin:$PATH"]){
                    // สร้าง Docker image จาก Dockerfile ที่อยู่ใน project
                    sh "docker build -t ${DOCKER_IMAGE} ."

                    // ลบ dangling images (images ที่ไม่มี tag)
                    sh "docker image prune -f"

                    // แสดงรายชื่อ Docker images ทั้งหมดที่มีอยู่หลังจาก build เสร็จ
                    sh "docker images"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                // Login เข้า Docker Hub และ push Docker image ที่สร้างขึ้น
                withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'DOCKERHUB_PASSWORD')]) {
                    sh "docker login -u knightboyz -p %DOCKERHUB_PASSWORD%"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withEnv(["PATH=/usr/local/bin:$PATH"]){
                    // ใช้ kubectl apply เพื่อ deploy หรืออัปเดต resource ใน Kubernetes จาก deployment.yaml
                    sh "kubectl --kubeconfig=${KUBE_CONFIG_PATH} apply -f deployment.yaml -n ${KUBE_NAMESPACE}"
                    
                    // ตรวจสอบสถานะของ deployment
                    sh "kubectl --kubeconfig=${KUBE_CONFIG_PATH} rollout status deployment/springboot-app -n ${KUBE_NAMESPACE}"
                }
            }
        }
    }

    post {
        always {
            // ทำความสะอาด workspace หลังการรัน pipeline เสร็จ
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}