node {
  withDockerContainer(args: '--name=python-alpine', image: 'python:2-alpine') {
    stage('Build') {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  withDockerContainer(args: '--name=python-qnib', image: 'qnib/pytest') {
    stage('Test') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      junit 'test-reports/results.xml'
    }
  }
  withDockerContainer(args:'--name=python-qnib -d', image: 'cdrx/pyinstaller-linux:python2') {
    stage('Deliver') {
      sh 'pyinstaller --onefile sources/add2vals.py'
    }
  }
}