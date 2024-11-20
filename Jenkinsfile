pipeline{
    agent any
        parameters{
            string(name: 'MYJOB' , defaultValue: '1.0.5')
        }
        environment{

        }
    stages{
         stage("scan file"){
            step{
                sh "trivy fs --format table -o job-portal.html"
            }
        }
        stage("Build image"){
            sh "docker build -t job-portal/ecr: ${params.MYJOB} ."
            sh "docker tag job-portal/ecr:${params.MYJOB} 727732356710.dkr.ecr.us-east-1.amazonaws.com/job-portal/ecr:${params.MYJOB} "
            sh "docker tag job-portal/ecr:${params.MYJOB} 727732356710.dkr.ecr.us-east-1.amazonaws.com/job-portal/ecr:latest"

        }
         stage ("scan image build"){
            step{
                sh "trivy image --format table -o docker_image_job-portal.html"
            }
        }
       
       
        stage(" push ECR"){
            step{
                script{
                    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'ecr:us-east-1:job-id', url: 'https://727732356710.dkr.ecr.us-east-1.amazonaws.com/') {

                    sh "docker push 727732356710.dkr.ecr.us-east-1.amazonaws.com/job-portal/ecr:${params.MYJOB}"
                    sh "docker push 727732356710.dkr.ecr.us-east-1.amazonaws.com/job-portal/ecr:latest"
                }
            }
            
        }
    }
    }
}