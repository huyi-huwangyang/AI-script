# ui-ai
UI through AI

# File structure
```
├── ai-test-script             # Code generator package
├── android-simulator          # Environment Setup instruction
└── test-builds                # This is where the build store, can be in other location
    └── dogscanner
        └── 202201142150
            ├── configs        # Config prepared by tester/system
            ├── Images         # Test data (Images and expected result)
            └── src            # Generated scripts
```

## ai-test-scripts
The user provides a basic test sctript for one test case and then it will convert the test script to a recursive script which will run all the test cases.

## test-builds
A sample of a test build, the path goes by `<app name>/<test build date>`  
There should be at least 2 folders prepared, configs and Images  
```
configs/
├── app_init.py               # Optional: Script recording to bypass first time opening tutorial
├── original_scripts.py       # Script recording to perform each test run
└── testConfig.py             # Simulator and app config
```

In testConfig.py there are 3 variables requried.
```
DESIRED_CAPS = {
    "platformName": 'Android',
    "deviceName": 'Samsung Galaxy S6',
    "avd": "samsung_galaxy_s6_8.1",
    "platfromVersion": "9.0.0",
    "browserName": "android",
    "automationName": "UIAutomator2",
    "autoGrantPermissions": True,                  # Auto accept photo and canera access
    # App config
    "app": "/root/tmp/dogscanner_2021-03-09.apk",  # Run dog Scanner
    "appWaitActivity": "com.siwalusoftware.*" ,
}

STARTAPP_STR = "com.siwalusoftware.dogscanner/com.siwalusoftware.scanner.activities.MainActivity"
CLOSEAPP_STR = "com.siwalusoftware.dogscanner"
```

**DESIRED_CAPS**: for device configuration, and app configuration.  
**STARTAPP_STR**: in order to get this, you can do it while recording the activity. In the main screen of the app run folling command
```
>adb shell dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp'
mCurrentFocus=Window{f0a6ddf u0 com.siwalusoftware.dogscanner/com.siwalusoftware.scanner.activities.MainActivity}
```
**CLOSEAPP_STR**: this will be the first part of STARTAPP_STR


# How to run

## Prepare the scripts
We use dogscanner as an example. 

### Environment setup
1. Follows [android-simulator](android-simulator/README.md) to setup container and Appium Inspector
### Optional: app init
2. Open the app and there is a first time user guide  
![1](https://user-images.githubusercontent.com/7858357/149636874-8f2ad36b-be74-456b-beab-bf801e7d225b.png)

3. Use the Appium Inspector to record the actions to complete the tutorial  
![2](https://user-images.githubusercontent.com/7858357/149636891-583e3adb-8a7d-47b4-b252-859003a720cd.png)  
![3](https://user-images.githubusercontent.com/7858357/149636906-05aaeea9-71b6-47db-baa9-7c8f64eea716.png)
4. Save it to configs/app_init.py
### Test script
5. Continue to use Appium Inspector to record the AI execution.  
![1](https://user-images.githubusercontent.com/7858357/149636915-7c3a3ce0-6dce-446f-be5f-00731b88e9ab.png)  
![2](https://user-images.githubusercontent.com/7858357/149636919-817bd5ab-60d7-4f1b-b1bb-6c5f07095dcf.png)
6. Save it to configs/original_scripts.py

## Prepare test data
Placeholder

## Run script generator
To run the script generator, go to `ai-test-scripts`  
then run `python3 convert.py <Path to build directory>`  
For example: `python3 convert.py ../test-builds/dogscanner/202201142150`

The script will generate test script based on files in `configs` and create script into `src`

```
src/
├── modified_scripts.py
└── rater.py
```

## Run test
To run test, go to the build path `test-builds/dogscanner/202201142150/src`  
Then run `python3 modified_script.py`

It will start running test based on test data from `Images`
