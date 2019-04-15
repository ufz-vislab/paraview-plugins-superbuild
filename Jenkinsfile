#!/usr/bin/env groovy
@Library('jenkins-pipeline@1.0.15') _

pipeline {
    agent { label "win1" }
    environment {
        MSVC_NUMBER = '15'
        MSVC_VERSION = '2017'
        FBX_ROOT = 'E:\\libs\\fbx\\2016.1.2-vs2015'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    configure {
                        sourceDir = 'paraview-superbuild'
                        cmakeOptions = '-DENABLE_ospray=ON -DENABLE_python=ON -DENABLE_qt5=ON -Dqt5_SOURCE_SELECTION=5.9 -DENABLE_paraviewpluginsexternal=ON -Dparaview_PLUGINS_EXTERNAL="FbxExporter;EnvimetReader" -Dparaview_PLUGIN_FbxExporter_PATH=../fbx-exporter -Dparaview_PLUGIN_EnvimetReader_PATH=../envimet-reader -Dqt5_EXTRA_CONFIGURATION_OPTIONS="-opengl;dynamic"'
                    }
                    build {}
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
