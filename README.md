
Sample application for Videokyc.

### Steps to use the SDK outlined below:
#### 1. settings.gradle :
```gradle
    pluginManagement {
        repositories {
            google()
            mavenCentral()
            jcenter()
            gradlePluginPortal()
        }
    }
    dependencyResolutionManagement {
        repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
        repositories {
            google()
            jcenter()
            mavenCentral()
            //enter github user name and github token
            maven {
                url = "https://maven.pkg.github.com/surepassio/videokyc-sample-app"
                credentials {
                    username = "USER_NAME"
                    password = "PAT_TOKEN"//https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token
                    // (Allow Package Read Permission in token)
                }
            }
    
    
        }
    }
```

#### 2. build.grade (app):
```groovy
   minSdk 26 //min sdk should be 26
   
   dependencies {
         implementation 'io.surepass.sdk:videokyc-android-sdk:1.0.0'
   }
```
Make sure to sync your project after adding the dependency.
#### 3. Inside Application:
```kotlin
   binding.btnGetStarted.setOnClickListener {
        val token = "YOUR TOKEN FROM API"
        openActivity(env, token)
    }
```
SDK will be started from openActivity function
```kotlin 
    private fun openActivity(env: String, token: String) {
        val intent = Intent(this, InitSDK::class.java)
        intent.putExtra("token", token)
        videoKycActivityResultLauncher.launch(intent)
    }
```
Response can be obtained in 
```kotlin 
   private fun registerActivityForResult() {
        videoKycActivityResultLauncher =
            registerForActivityResult(
                ActivityResultContracts.StartActivityForResult(),
                ActivityResultCallback { result ->
                    val resultCode = result.resultCode
                    val data = result.data
                    if (resultCode == RESULT_OK && data != null) {
                        val videoKycResponse = data.getStringExtra("result")
                        Log.e("MainActivity", "video Response $videoKycResponse")
                        showResponse(videoKycResponse)
                    }
                })
    }
```
For better clarification you can check the code details inside the project

#### 4. Rebuild your project:
   After making changes to the theme, rebuild your project to apply the updated theme throughout your application.
