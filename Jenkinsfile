#!/usr/bin/env groovy

@Library("<jenkins_library>") _


pipeline {
  agent {
    label 'docker-maven-slave'
  }

  //ENVIRONMENTAL VARIABLE DECLARATIONS:
  environment {
    USER_ID = '<USER_ID>'
   CREDENTIALS = '<credentials>' //[ Credentials for <project> For GitHub | Artifactory | K8]

    // EMAIL_ID = null
    APPROVAL_AUDIENCE = "'<email_id>'"
    NOTIFICATION_AUDIENCE = "'<email_id>'"
  }

  // private repo 
  stages {
    /*   
    //APPROVE DEPLOYMENT:   
    stage ('Approve Deployment') {
        when { expression { (env.GIT_BRANCH == 'master') || (env.GIT_BRANCH.contains('release/')) } }
        steps {
            script {
                emailext mimeType: 'text/html',
                subject: "[Jenkins]${currentBuild.fullDisplayName} deploy to ${env.GIT_BRANCH} Approval",
                to: "${env.APPROVAL_AUDIENCE}",
                body: '''<a href="${BUILD_URL}input">click to approve</a>
                <p>Cause: "${BUILD_CAUSE}"</p>'''
                def userInput = input(
                id: 'userInput',
                ok: 'Yes',
                message: 'Yes to Approve, Abort to reject',
                parameters: [booleanParam(defaultValue: true, description: 'Approve deploy',name: 'Approve?')])
            }
        }
    }*/
    //BUILD JAR:
    // stage ('Build JAR') {
    //  steps {
    //      script {
    //          if ((env.GIT_BRANCH == 'master') || (env.GIT_BRANCH.contains('release/'))) {
    //              archiveArtifacts 'Jenkinsfile,Optumfile.yml'
    //              glMavenBuild [:]
    //              EMAIL_ID = "${env.NOTIFICATION_AUDIENCE}"
    //          }
    //      }
    //  }
    // }

    //FOR FUTURE PURPOSE:
    /* stage('Sonar Scan') {
        steps {
          glSonarMavenScan gitUserCredentialsId:"${env.GITHUB_USER}"
        }
     } */

    //TAGGING AND PUSHING THE IMAGE ON ARTIFACTORY
    stage('Preparing, Tagging and Pushing Docker Image on Artifactory - Core spark Setup') {
      steps {
        script {
          if (env.GIT_BRANCH == 'master') {

            withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
              sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
              sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=master --build-arg sparkDefaultsConfig=spark-defaults-prod.conf -t dqaas-prod:dqcml-prod-Spark-core -f Dockerfile_SparkCore_DAC ."
              sh "/tools/docker/docker-17.12.0/docker tag dqaas-prod:dqcml-prod-Spark-core <repository_hub_name>/dqaas-prod:dqcml-prod-Spark-core"
              sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/dqaas-prod:dqcml-prod-Spark-core"
              echo "Docker Image For [Spark-core][PROD] is Tagged and is Pushed on Artifactory"
            }

          } else {
            if (env.GIT_BRANCH == 'snowflake_connector') {
              // withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
              //   sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
              //   sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=snowflake_connector  --build-arg sparkDefaultsConfig=spark-defaults.conf -t <USER_ID>:dqcml-nonprod-Spark-baseSetup -f Dockerfile_spark_setup ."
              //   sh "/tools/docker/docker-17.12.0/docker tag <USER_ID>:dqcml-nonprod-Spark-baseSetup <repository_hub_name>/<USER_ID>/<USER_ID>:dqcml-nonprod-Spark-baseSetup"
              //   sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/<USER_ID>/<project>:dqcml-nonprod-Spark-baseSetup"
              //   echo "Docker Image For [Spark-Base-setup][NON-PROD] is Tagged and is Pushed on Artifactory"
              // }

              withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
                sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=snowflake_connector  --build-arg sparkDefaultsConfig=spark-defaults.conf -t <project>:dqcml-nonprod-Spark-core -f Dockerfile_SparkCore_DAC ."
                sh "/tools/docker/docker-17.12.0/docker tag <USER_ID>:dqcml-nonprod-Spark-core <repository_hub_name>/<project>:dqcml-nonprod-Spark-core"
                sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/<USER_ID>:dqcml-nonprod-Spark-core"
                echo "Docker Image For [Spark-core][NON-PROD] is Tagged and is Pushed on Artifactory"
              }
            }

          }
        }
      }
    }

    stage('Preparing, Tagging and Pushing Docker Image on Artifactory - Backend DAC logic') {
      steps {
        script {
          if (env.GIT_BRANCH == 'master') {

            // withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            //   sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
            //   sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=master --build-arg sparkDefaultsConfig=spark-defaults-prod.conf -t dqaas-prod:dqcml-prod-DAC -f Dockerfile_DAC ."
            //   sh "/tools/docker/docker-17.12.0/docker tag dqaas-prod:dqcml-prod-DAC <repository_hub_name>/dqaas-prod:dqcml-prod-DAC"
            //   sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/dqaas-prod:dqcml-prod-DAC"
            //   echo "Docker Image For [DAC][PROD] is Tagged and is Pushed on Artifactory"

            // }

          } else {
            if (env.GIT_BRANCH == 'snowflake_connector') {
              withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
                sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=snowflake_connector --build-arg sparkDefaultsConfig=spark-defaults.conf -t <USER_ID>:dqcml-nonprod-BaseSetup-DAC -f Dockerfile_DAC_BaseSetup ."
                sh "/tools/docker/docker-17.12.0/docker tag <USER_ID>:dqcml-nonprod-BaseSetup-DAC <repository_hub_name>/<USER_ID>/<USER_ID>:dqcml-nonprod-BaseSetup-DAC"
                sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/<USER_ID>:dqcml-nonprod-BaseSetup-DAC"
                echo "Docker Image For [DAC-SparkBase][NON-PROD] is Tagged and is Pushed on Artifactory"
              }

              withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
                sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=snowflake_connector --build-arg sparkDefaultsConfig=spark-defaults.conf -t <USER_ID>:dqcml-nonprod-DAC -f Dockerfile_DAC ."
                sh "/tools/docker/docker-17.12.0/docker tag <USER_ID>:dqcml-nonprod-DAC <repository_hub_name>/<USER_ID>:dqcml-nonprod-DAC"
                sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/<USER_ID>:dqcml-nonprod-DAC"
                echo "Docker Image For [DAC-Spark][NON-PROD] is Tagged and is Pushed on Artifactory"
              }
            }

          }
        }
      }
    }

    // stage('Preparing, Tagging and Pushing Docker Image on Artifactory - tfs') {
    //  steps {
    //      script {
    //                  if (env.GIT_BRANCH == 'master') {

    //                          withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
    //                              sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
    //             sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=master --build-arg sparkDefaultsConfig=spark-defaults-prod.conf -t dqaas-prod:dqcml-prod-tfs -f Dockerfile_tfs_Production ."
    //             sh "/tools/docker/docker-17.12.0/docker tag dqaas-prod:dqcml-prod-tfs <repository_hub_name>/dqaas-prod:dqcml-prod-tfs"
    //             sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/dqaas-prod:dqcml-prod-tfs"
    //              echo "Docker Image For [tfs][PROD] is Tagged and is Pushed on Artifactory"
    //          }

    //          }

    //          else {
    //          if (env.GIT_BRANCH == 'snowflake_connector'){
    //              withCredentials([usernamePassword(credentialsId: '<credentials>', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
    //             sh "/tools/docker/docker-17.12.0/docker login --username '${DOCKER_USERNAME}' --password '${DOCKER_PASSWORD}' <repository_hub_name>"
    //             sh "/tools/docker/docker-17.12.0/docker build --build-arg gitBranch=snowflake_connector --build-arg sparkDefaultsConfig=spark-defaults.conf -t <USER_ID>:dqcml-nonprod-tfs -f Dockerfile_tfs_NonProduction ."
    //             sh "/tools/docker/docker-17.12.0/docker tag <USER_ID>:dqcml-nonprod-tfs <repository_hub_name>/<USER_ID>:dqcml-nonprod-tfs"
    //             sh "/tools/docker/docker-17.12.0/docker push <repository_hub_name>/<USER_ID>:dqcml-nonprod-tfs"
    //              echo "Docker Image For [tfs][NON-PROD] is Tagged and is Pushed on Artifactory"
    //          }
    //          }
    //          }
    //      }
    //  }
    // }

    //DEPLOY TAGGED DOCKER IMAGE ON KUBERNERTES[OSFI]
    stage('Deployiong on OSFI - web ') {
      //agent{ label 'docker-kitchensink-slave' }
      steps {
        script {
          if (env.GIT_BRANCH == 'master') {
            glKubernetesApply credentials: "dqc-prod-token",
              cluster: "k8s-prod-ctc-aci.optum.com",
              namespace: "<prod-namespace>",
              yamls: ["Services_Production_web.yml", "Deployment_Production_web.yml", "Network_prod.yml", "Ingress_prod.yml"], //["Services_Production_web.yml","Deployment_Production_web.yml"],
              // deploymentYaml: "Deployment_Production_web.yml",
              isProduction: true,
              env: "prod",
              deleteIfExists: true, wait: false
          } else {
            if (env.GIT_BRANCH == 'snowflake_connector') {
              glKubernetesApply credentials: "dqc_nonprod_token",
                cluster: "k8s-prod-ctc-aci.optum.com",
                namespace: "<dev-namespace>",
                yamls: ["Deployment_NonProduction_web.yml", "Services_NonProduction_web.yml", "Network_nonprod.yml", "Ingress_nonprod.yml"], //"Services_NonProduction_web.yml",
                // deploymentYaml: "Deployment_NonProduction_web.yml",
                isProduction: false,
                env: "nonprod",
                deleteIfExists: true, wait: false
            }
          }
        }
      }
    }

    // stage('Deployiong on OSFI - tfs '){
    //  //agent{ label 'docker-kitchensink-slave' }
    //  steps {
    //      script {
    //          if (env.GIT_BRANCH == 'master') {
    //              glKubernetesApply credentials: "dqc-prod-token",
    //              cluster: "k8s-prod-ctc-aci.optum.com",
    //              namespace: "<prod-namespace>",
    //              yamls: ["Services_Production_tfs.yml","Deployment_Production_tfs.yml"],// there should be 4 yamls here
    //              // deploymentYaml: "Deployment_Production_tfs.yml",
    //              isProduction: true,
    //              env:"prod", 
    //              deleteIfExists: true, wait: false
    //          } else {
    //              if (env.GIT_BRANCH == 'snowflake_connector'){
    //                  glKubernetesApply credentials: "dqc_nonprod_token",
    //                  cluster: "k8s-prod-ctc-aci.optum.com",
    //                  namespace: "<dev-namespace>",
    //                  yamls: ["Deployment_NonProduction_tfs.yml","Services_NonProduction_tfs.yml"], //"Services_NonProduction_tfs.yml",
    //                  isProduction: false,
    //                  env: "nonprod", 
    //                  deleteIfExists: true, wait: false
    //              }
    //          }
    //      }
    //  }
    // }

  }

  //POST DECLARATIVE ACTIONS:
  post {
    success {
      echo 'The pipeline is marked as success'
      emailext body: "Build URL: ${BUILD_URL}",
        subject: "Successful Build: ${env.GIT_BRANCH}",
        to: "'<email_id>'"
    }
    failure {
      echo 'The pipeline is marked as failed'
      emailext body: "Build URL: ${BUILD_URL}",
        subject: "Build Failure: ${env.GIT_BRANCH}",
        to: "'<email_id>'"
    }
    unstable {
      echo 'The pipeline is marked as unstable'
      emailext body: "Build URL: ${BUILD_URL}",
        subject: "Unstable Build: ${env.GIT_BRANCH}",
        to: "'<email_id>'"
    }
  }
}
