pipeline {
    agent { label "win1" }
    environment {
        CC = 'cl.exe'
        CXX = 'cl.exe'
        FBX_ROOT = 'E:\\libs\\fbx\\2016.1.2-vs2015'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    bat """
                        cd paraview-superbuild\\superbuild
                        git fetch origin
                        git checkout master
                        cd ..\\..
                        REM In windows bat there is no (!) command for moving
                        REM hidden folders!!!!
                        powershell -Command "mv .git .git-bak"
                        call "%VS150COMNTOOLS%\\..\\..\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                        mkdir build
                        cd build
                        cmake ../paraview-superbuild -G Ninja -DCMAKE_BUILD_TYPE=Release -DENABLE_ospray=ON -DENABLE_python=ON -DENABLE_qt5=ON -Dqt5_SOURCE_SELECTION=5.9 -DENABLE_paraviewpluginsexternal=ON -Dparaview_PLUGINS_EXTERNAL='FbxExporter;EnvimetReader' -Dparaview_PLUGIN_FbxExporter_PATH=../fbx-exporter -Dparaview_PLUGIN_EnvimetReader_PATH=../envimet-reader -Dqt5_EXTRA_CONFIGURATION_OPTIONS="-opengl;dynamic"
                        ninja
                        cd ..
                        powershell -Command "mv .git-bak .git"
                    """.stripIndent()
                }
            }
        }
        stage('Package') {
            steps {
                dir('build') {
                    bat "ctest -R cpack"
                }
            }
        }
    }
    post {
        success {
            archiveArtifacts "**/*.zip"
        }
    }
}
