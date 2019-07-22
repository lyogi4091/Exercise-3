properties([parameters([booleanParam(defaultValue: true, description: 'select to format the code', name: 'Autopep8')])])
node {
    stage('Building Docker image'){
        git 'git@github.com:lyogi4091/Exercise-3.git'
        sh 'sudo docker build -t ubuntu_image_remotely .'
        sh 'sudo docker run -it -d --name testcontainer_remote_exercise_3 -v /home/ciuser/Exercise-3:/opt ubuntu_image_remotely'
        try{
            sh 'sudo docker exec testcontainer_remote_exercise_3 cat /opt/python_good.py'
            sh 'sudo docker exec testcontainer_remote_exercise_3 pycodestyle --ignore=E501 /opt/python_good.py'
            println "Code is already in good format"
            }catch(x){
                if (Boolean.parseBoolean("${env.Autopep8}")) {
                    sh 'sudo docker exec testcontainer_remote_exercise_3 autopep8 -i /opt/python_good.py'
                    sh 'sudo docker exec testcontainer_remote_exercise_3 cat /opt/python_good.py'
                    println "Code is now formatted."
                }
                }
        try{
            sh 'sudo docker exec testcontainer_remote_exercise_3 cat /opt/python_bad.py'
            sh 'sudo docker exec testcontainer_remote_exercise_3 pycodestyle --ignore=E501 /opt/python_bad.py'
            println "Code is already in good format"
            }catch(x){
                if (Boolean.parseBoolean("${env.Autopep8}")){
                    sh 'sudo docker exec testcontainer_remote_exercise_3 autopep8 -i /opt/python_bad.py'
                    sh 'sudo docker exec testcontainer_remote_exercise_3 cat /opt/python_bad.py'
                    println "Code is now formatted"
                }
                }
    stage('Pushing the formatted code to GitHub'){
        dir('~/Exercise-3'){
            try{
                sh 'git config --global user.email "lingojuyogesh.kumar@ltts.com"';
                sh 'git config --global user.name "Yogesh Kumar"'
                sh 'git add python*.py';
                sh 'git commit -m "Commit after auto-format of code"';
                sh 'git push origin master';
		println "Code is pushed successfully.";
                }catch(n){
                    println "Code is already in good format. So, nothing to push."
                }
        }
    }
}
}
