pipeline {
     agent { 
        kubernetes{
            label 'jenkins-slave'
        } 
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }
 
    
    environment { 
        art = 'artefacto'
        VERSION = '0.0.0.0.1'
    }

    stages {
        stage('Enviroment DEV') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'feature/*'
                    branch 'hotfix/*'
            branch 'main'
                }
            }
            steps {
        sleep 5    
                script {
                    AMBIENTE = 'develop' 
                    echo "${AMBIENTE}"                                  
                }
            }
        }

        stage('Ambiente Trunk') {
            when {
                anyOf {
                    branch 'release/*'
                }
            }
            steps {
        sleep 10    
                script {
                    AMBIENTE = 'qa' 
                    echo "${AMBIENTE}"           
                }
            }
        }

        stage('Ambiente Pro') {
            when {
                anyOf {
                    branch 'master'
                }
            }
            steps {
                script {
                    AMBIENTE = 'prd' 
                    echo "${AMBIENTE}"                
                }
            }
        }

        stage("Unit Tests Execution") {
            when {
                anyOf {
                    branch "develop" 
                }
            }
            steps {
                sh "npm run test"                
            }
        }

     
        stage('SonarQube Review') {
           when {
                 anyOf {
                     branch "develop" 
                     branch "release/*"                   
                 }
              }
            steps { 
            sleep 10
            script {
                echo " ...sona Done"
               }
            }
        }

        stage('Config Environment DEV') {
            when {
                 anyOf {
                     branch "develop"
                 }
              }
            steps {
                script {
                    echo " ...done..."
                }
            }
        }

        stage('Config Env Trunk') {
            when {
                 anyOf {
                     branch "release/*"
                 }
              }
            steps {
                script {
                    echo " ...config trunk done..."
                }
            }
        }

        stage('Config Environment pro') {
            when {
                 anyOf {
                     branch "master"
                 }
              }
            steps {
                script {
                    echo " ...config Pro done..."
                }
            }
        }  
        
        
        stage('Build Image') {
            when {
                 anyOf {
                     branch "develop"
                     branch "release/*"
                     branch "master"
                 }
              }
            steps {
                script {
                    echo 'Init Build Image'  
                    echo 'End Build Image'
                }
            }
        }
        
        stage("Deploy DEV") {
            when {
                anyOf {
                    branch "develop"
                }
            }   
           
            steps {
               script {
                    echo "Init Deploy Image to DEV Environment"
                    echo 'Image Done'  
                }
            }
        }
             
        stage("Deploy QA") {
            when {
                anyOf {
                    branch "release/*"
                }
            }   
            
            options { 
                 timeout(time: 1, unit: 'HOURS')
             }   
             
            steps {
                script {
                    echo "Init Deploy Image to Trunk Environment"                
                }
            }
        }

        stage('DAST') {
            when {
                anyOf {
                    branch "develop"
                    branch "release/*"
                }
            }  
            steps {
               script {
                    echo ' branch: ' + env.BRANCH_NAME 
                 
               }
            }
        }
        
        stage("Deploy PRD") {
            when {
                anyOf {
                    branch "master"
                }
            }   
                  
            steps {
                script {
                    echo "Init Deploy Image to PRD Environment"
                    echo "....pro Done!!!"
                   
                }
            }
        }
    }
} 
