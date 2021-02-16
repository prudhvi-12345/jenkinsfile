node () {
    stage("parameters"){
        properties([parameters([choice(choices: ['dev', 'test'], description: '', name: 'envorinment')])])
    }
    stage("git clone"){
        git 'https://github.com/bhanuprakash678910/mavenproj.git'
    }
    stage ("build stage"){
       sh label: '', script: 'mvn clean package' 
    }
    stage("code quality"){
       sh label: '', script: '''mvn sonar:sonar \\
  -Dsonar.projectKey=sample \\
  -Dsonar.host.url=http://54.160.85.242:9000 \\
  -Dsonar.login=5030e26843a3347202558dcbc808e953aeb8650a''' 
    }
    stage("storing artifacts in s3"){
        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'jenkinsartifactstorage', excludedFile: 'webapp/target', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-iso-east-1', showDirectlyInBrowser: false, sourceFile: '**/webappp/target/**.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'prudhvi', userMetadata: []
    }
    stage("send email"){
        emailext body: 'ur build is succesfull', recipientProviders: [developers()], subject: 'about jenkins build', to: 'prudhvi.v117@gmail.com'
    }
}
