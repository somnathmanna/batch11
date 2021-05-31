try{
    node{
        def mavenHome
        def mavenCMD
        def docker
        def dockerCMD
        def tagName = "1.4"
        
        stage('Preparation'){
            echo "Preparing the Jenkins environment with required tools..."
            mavenHome = tool name: 'maven 3', type: 'maven'
            mavenCMD = "${mavenHome}/bin/mvn"
            docker = tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
            //dockerCMD = "$docker/bin/docker"
        }
        
        stage('git checkout'){
            echo "Checking out the code from git repository."
            git 'https://github.com/tarun3834/batch10.git'
                             }
        
        stage('Build, Test and Package'){
            echo "Building the springboot application."
            sh "${mavenCMD} clean package"
            //sh "java -jar target/my-test-app*.jar"
                                      }
        stage('Sonar check'){
            echo "scanning the app"
            tool name:'maven 3',type: 'maven'
            sh 'mvn sonar:sonar -Dsonar.projectKey=sonar -Dsonar.login=admin -Dsonar.password=admin123 -Dsonar.sources=/var/lib/jenkins/workspace/case -Dsonar.host.url=http://34.69.89.128:9000'
        }            
              
        stage('Build Docker Image'){
            echo "Building docker image for springboot application."
            sh "docker build -t tarun3834/spring:${tagName} ."
            }
        stage("Push Docker Image to Docker Registry"){
            echo "Pushing image to docker hub"
            withCredentials([string(credentialsId: 'dockerPwd', variable: 'dockerHubPwd')]) {
            sh "docker login -u tarun3834 -p ${dockerHubPwd}"
            }
            sh "docker push tarun3834/spring:${tagName}"
        }
    }
}
catch(Exception err){
    
}
finally{
    
}
