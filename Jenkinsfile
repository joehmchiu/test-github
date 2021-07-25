pipeline {
  environment {
    // create a backup workspace
    WS = '/opt/projects/mongo-api'
    TC = 50
    crudxml = '/tmp/crud-test.xml'
    loadxml = '/tmp/load-test.xml'
    tmpfile = '/tmp/test-tmp-file'
    testfile = '/tmp/test-results'
  }

  agent any
  options {
      timeout(time: 1, unit: 'HOURS') 
  }

  stages {
    stage('Ready for Release') {
      steps {
        sh '''#!/bin/bash
          echo "9. Tag for release ready"
          sudo cp -p /opt/devops/mongo-api/vault ./group_vars/all/
          sudo ansible-playbook -T 120 uat-release.yml -vvv
          sudo rm -f ./group_vars/all/vault
          echo "10. Release tagged!"
        '''
        sh "cp -f /tmp/*.xml '$WORKSPACE'/reports/."
      }
    }
  }
  post {
    always {
      junit allowEmptyResults: true, testResults: '**/reports/*.xml'
    }
  }
}
