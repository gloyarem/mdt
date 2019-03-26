node (sonarcube){
    stage ('Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
            doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
            userRemoteConfigs: [[url: 'https://github.com/gloyarem/mdt.git']]])
    }
    stage ('Build') {
        nodejs('Node10') {
            sh '''
                cd $WORKSPACE/www/css
                cleancss -d style.css > ../min/cutom-min.css

                cd $WORKSPACE/www/js
                uglifyjs --timings init.js -o ../min/custom-min.js

                cd $WORKSPACE
                tar --exclude='./www/css' --exclude './www/js' -c -z -f archive.tgz ./www/

                echo $SSH_FILE
                echo $SSH_USER_NAME
                echo $PARAM_VERSION
            '''
        }
    }
    stage ('Archive') {
        archiveArtifacts 'archive.tgz'
    }
}
