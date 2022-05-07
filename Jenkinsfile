pipeline {
	agent any
		stages {
			stage("Building the student survey image") {
				steps {
					script {
						checkout scm
						sh 'rm -rf *.war'
						sh 'jar -cvf Ishana_SWE_HW1.war -C WebContent/ .'
						sh 'echo ${BUILD_TIMESTAMP}'
						sh "docker login -u ishana7 -p Bts@71611"
						def customImage = docker.build("ishana7/swe_hw2:0.0.1:${BUILD_TIMESTAMP}")
					}
				}
			}
			stage("Pushing Image to DockerHub") {
				steps {
					script {
						sh 'docker push ishana7/swe_hw2:0.0.1:${BUILD_TIMESTAMP}'
					}
				}
			}
			
			stage("Deploying to Rancher as single pod") {
				steps {
				   sh 'kubectl set image deployment/swecheck/swecheck2 container-0=ishana7/swe_hw2:0.0.1:${BUILD_TIMESTAMP} -n swehw2_final2'
				  }
			}
			
			stage("Deploying to Rancher as with load balancer") {
				steps {
				   sh 'kubectl set image deployment/swecheck2-loadbalancer swecheck2-loadbalancer=ishana7/swe_hw2:0.0.1:${BUILD_TIMESTAMP} -n swehw2_final2'
				  }
			   }
			}
		}
   
