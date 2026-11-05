pipeline {
    agent any
    tools {
        // Jenkins లో మీరు ఏ పేరుతో అయితే NodeJS టూల్ ని add చేశారో ఆ పేరు ఇవ్వండి
        nodejs 'node23' 
    }
    
    environment {
        AWS_REGION = "ap-southeast-2"
        
        // మార్చాలి: మీ 12 అంకెల AWS అకౌంట్ ఐడీ ఇక్కడ ఇవ్వండి
        AWS_ACCOUNT_ID = "232723758111" 
        
        ECR_REPO = "bms-ecr"
        CLUSTER_NAME = "bookmyshow-cluster"
        
        // మార్చాలి: మీ గిట్‌హబ్ క్రెడెన్షియల్స్ ఐడీ మరియు మీ రిపో లింక్ ఇవ్వండి
        GIT_CRED_ID = "git-cred"
        GIT_URL = "https://github.com/mahalaxmi1308/Book-My-Show.git"
        
        // ECR పూర్తి లింక్ ఆటోమేటిక్ గా క్రియేట్ అవుతుంది
        IMAGE_URI = "232723758111.dkr.ecr.ap-southeast-2.amazonaws.com/bms-ecr"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                // ఇక్కడ మీ గిట్ క్రెడెన్షియల్స్ ఆటోమేటిక్ గా వాడబడతాయి
                git branch: 'eks-pipeline', credentialsId: "${GIT_CRED_ID}", url: "${GIT_URL}"
            }
        }
    
       stage('Install Node Dependencies') {
    steps {
        dir('bookmyshow-app') {
            sh 'npm install'
        }
    }
}
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_URI} ./bookmyshow-app"
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
                // IAM రోల్ ద్వారా AWS కి లాగిన్ అవ్వడం
                sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME}"
                
                // ఇక్కడ ఈ కొత్త లైన్ యాడ్ చేయండి (ఇది ఫైల్స్ లిస్ట్‌ని ప్రింట్ చేస్తుంది):
                sh "ls -la" 
                
                // కుబెర్నెటీస్ లో డిప్లాయ్ చేయడం
                sh "kubectl apply -f deployment.yml"
            }
        }
    }
}
