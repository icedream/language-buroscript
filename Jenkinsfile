/*
Jenkins CI script.
Will be run at https://ci.icedream.pw/.
*/

def runInsideDocker(image, f) {
  node("linux && x64 && docker") {
    docker.image(image).inside(f)
  }
}

def prepareBuild(os) {
  switch(os) {
    case "debian":
    case "ubuntu":
      sh "apt-get update"
      sh "apt-get install -y curl"
    break
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
      runInsideDocker("debian") {
        withEnv([
          "TRAVIS_OS_NAME=linux",
          "DEBIAN_FRONTEND=noninteractive",
        ]) {
          prepareBuild("debian")
          checkout scm
          buildPackage("stable")
        }
      }
    },

    "os=debian, atom=beta": {
      runInsideDocker("debian") {
        withEnv([
          "TRAVIS_OS_NAME=linux",
          "DEBIAN_FRONTEND=noninteractive",
        ]) {
          prepareBuild("debian")
          checkout scm
          buildPackage("beta")
        }
      }
    },
  )
}
