pipeline {
  agent {
    docker {
      image 'golang:1.9.2'
    }
  }

  stages {
    stage('Test') {
      steps {
        sh """
        rm -f $WORKSPACE/test-results.{log,xml}
        mkdir -p /go/src/github.com/toddboom
        cp -a $WORKSPACE /go/src/github.com/toddboom/influxql

        cd /go/src/github.com/toddboom/influxql
        go get -v -t
        go test -v | tee $WORKSPACE/test-results.log
        """
      }

      post {
        always {
          sh """
          if [ -e test-results.log ]; then
            go get github.com/jstemmer/go-junit-report
            go-junit-report < $WORKSPACE/test-results.log > test-results.xml
          fi
          """
          junit "test-results.xml"
        }
      }
    }
  }
}
