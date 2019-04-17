#!/usr/bin/env groovy
@Library('jenkins-pipeline@1.0.21') _

pipeline {
    agent {
        node {
            label "win1"
            customWorkspace "E:\\_pv"
        }

    }
    environment {
        MSVC_NUMBER = '15'
        MSVC_VERSION = '2017'
        FBX_ROOT = 'E:\\libs\\fbx\\2016.1.2-vs2015'
    }
    options { skipDefaultCheckout() }
    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'src'], [$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: true, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '2719b702-1298-4e87-8464-5dfc62fbd923', url: 'https://github.com/ufz-vislab/paraview-plugins-superbuild.git']]])
                script {
                    configure {
                        sourceDir = 'src/paraview-superbuild'
                        cmakeOptions = '-DENABLE_ospray=ON -DENABLE_python=ON -DENABLE_qt5=ON -Dqt5_SOURCE_SELECTION=5.9 -DENABLE_paraviewpluginsexternal=ON -Dparaview_PLUGINS_EXTERNAL="FbxExporter;EnvimetReader" -Dparaview_PLUGIN_FbxExporter_PATH=../src/fbx-exporter -Dparaview_PLUGIN_EnvimetReader_PATH=../src/envimet-reader -Dqt5_EXTRA_CONFIGURATION_OPTIONS="-opengl;dynamic"'
                    }
                    build { }
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
