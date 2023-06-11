node {
  withDockerContainer(image: 'python:2-alpine') {
    stage('Build') {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  withDockerContainer(image: 'qnib/pytest') {
    stage('Test') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      junit 'test-reports/results.xml'
    }
  }
  withEnv([
    'VOLUME=${pwd}/sources:/src',
    "IMAGE=cdrx/pyinstaller-linux:python2"
  ]) {
    stage('Deliver') {
      sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
    }
  }
}