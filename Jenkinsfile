#!groovy

/*
 * Copyright © 2017 IBM Corp. All rights reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file
 * except in compliance with the License. You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software distributed under the
 * License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
 * either express or implied. See the License for the specific language governing permissions
 * and limitations under the License.
 */

def buildAndTest(nodeLabel) {
  node(nodeLabel) {
      checkout scm
      withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'clientlibs-test', usernameVariable: 'TEST_COUCH_USERNAME', passwordVariable: 'TEST_COUCH_PASSWORD']]) {
        withEnv(["TEST_COUCH_URL=https://clientlibs-test.cloudant.com"]) {
          sh 'swift build'
          // Workaround Xcode 9 issues with 'swift test' command failing
          sh 'swift package generate-xcodeproj'
          sh 'xcodebuild -project SwiftCloudant.xcodeproj -scheme SwiftCloudant-Package test'
          // sh 'swift test'
        }
      }
  }
}


stage('QA') {
    parallel(
        Mac:
        {
          buildAndTest("macos")
        }
        // Uncomment for Linux Testing Support
        // ,
        // Linux:
        // {
        //     buildAndTest(null)
        // }
    )
  }
