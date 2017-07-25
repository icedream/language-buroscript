/*
Jenkins CI script.
Will be run at https://ci.icedream.pw/.
*/

def runInsideDocker(image, f) {
  node("linux && x64 && docker") {
    docker.image(image).inside(f)
  }
}

def buildPackage(atomChannel) {
  withEnv([
    "ATOM_CHANNEL=${atomChannel}",
  ]) {
    sh """
    curl -s -O https://raw.githubusercontent.com/atom/ci/master/build-package.sh
    chmod u+x build-package.sh
    ./build-package.sh
    """
  }
}

node("linux && x64 && docker") {
  parallel (
    "os=debian, atom=stable": {
      withEnv([
        "TRAVIS_OS_NAME=linux",
      ]) {
        checkout scm
        buildPackage()
      }
    },

    "os=debian, atom=beta": {
      withEnv([
        "TRAVIS_OS_NAME=linux",
      ]) {
        checkout scm
        buildPackage()
      }
    },
  )
}
