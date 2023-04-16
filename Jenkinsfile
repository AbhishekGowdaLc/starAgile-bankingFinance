node {
    def mavenHome
    def mavenCMD
    def docker
    def dockerCMD
    def tagName
    
    stage('prepare environment'){
        echo 'Initialize the variables'
        mavenHome = tool name: 'MAVEN_HOME' , type: 'maven'
        mavenCMD = "${mavenHome}/bin/mvn"
        docker = tool name: 'MY_DOCKER' , type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        dockerCMD = "${docker}/bin/docker"
        tagName = "1.0"
    }
    stage ('code checkout'){
        try{
        echo 'pulling the code from github repo'
        git 'https://github.com/AbhishekGowdaLc/banking-domain-project'
        }
        catch(Exception e){
            echo 'Exception Occur'
        }
    stage('Build the application'){
        echo 'clean and compile and test package'
        //sh 'mvn clean package'
        sh "${mavenCMD} clean package"
    }
    stage('publish html reports'){
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/StrarAgileDevopsPipeline/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report Staragile/var/lib/jenkins/workspace/StrarAgileDevopsPipeline', reportTitles: '', useWrapperFileDirectly: true])
    }
    stage('Build the DockerImage of the application'){
        try{
        echo 'creating the docker image'
		// if you get permission denied issue
        //sudo usermod -a -G docker jenkins
        //restart Jenkins
        //or add sudoers file below line
        //jenkins ALL=(ALL) NOPASSWD:ALL
        sh "${dockerCMD} build -t abhishekgowda123/bankingproject:${tagName} ."
        
        }
        catch(Exception e){
            echo 'Exception Occur'
        }
    }
    stage('push the docker image to dockerhub'){
        echo 'pushing docker image'
        withCredentials([string(credentialsId: 'docker-password', variable: 'DockerPassword')]) {
        // some block
        sh "${dockerCMD} login -u abhishekgowda123 -p ${DockerPassword}"
        sh "${dockerCMD} push abhishekgowda123/bankingproject:${tagName}"
        }
    }
}

 
