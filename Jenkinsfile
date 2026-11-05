pipeline {
    agent any
    
    environment {
        AWS_REGION = "ap-southeast-2"
        
        // మార్చాలి: మీ 12 అంకెల AWS అకౌంట్ ఐడీ ఇక్కడ ఇవ్వండి
        AWS_ACCOUNT_ID = "123456789012" 
        
        ECR_REPO = "bookmyshow"
        CLUSTER_NAME = "bookmyshow-cluster"
        
        // మార్చాలి: మీ గిట్‌హబ్ క్రెడెన్షియల్స్ ఐడీ మరియు మీ రిపో లింక్ ఇవ్వండి
        GIT_CRED_ID = "github-creds"
        GIT_URL = "https://github.com/your_username/bookmyshow.git"
        
        // ECR పూర్తి లింక్ ఆటోమేటిక్ గా క్రియేట్ అవుతుంది
        IMAGE_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:latest"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                // ఇక్కడ మీ గిట్ క్రెడెన్షియల్స్ ఆటోమేటిక్ గా వాడబడతాయి
                git branch: 'main', credentialsId: "${GIT_CRED_ID}", url: "${GIT_URL}"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_URI} ."
            }
        }
        
        stage('Push to AWS ECR') {
            steps {
                // IAM రోల్ ద్వారా AWS లో లాగిన్ అయ్యి ECR కి ఇమేజ్ పుష్ చేయడం
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                sh "docker push ${IMAGE_URI}"
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                // IAM రోల్ ద్వారా EKS కి లాగిన్ అవ్వడం
                sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME}"
                // కుబర్నెటీస్ ఫైల్ ని అప్లై చేయడం
                sh "kubectl apply -f k8s-deployment.yaml"
            }
        }
    }
}
