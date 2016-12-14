"""
To use this script

1. Install rake and bundler 

```
  sudo gem install rake
  sudo gem install bundler
``` 

- Install gem dependencies

```
  cd path\to\WebDriverAgentSourceCode
  bundle install
```

- How to see all tasks:

```
  rake -T
```

"""
require 'rubygems'  

project_file = "WebDriverAgent.xcodeproj"
scheme_name = "WebDriverAgentRunner"
configuration = "Debug"
derivedDataPath = "./derivedData"
final_App_Name = "WebDriverAgentRunner-Runner"
zipPath = "./#{final_App_Name}*.zip"

def run(command, min_exit_status = 0)
  puts "Executing: `#{command}`"
  system(command)
  return $?.exitstatus
end

def xcodebuild(flags , logfile)
  cmd = "set -o pipefail && xcrun xcodebuild #{flags} | tee #{logfile} | xcpretty --color"
  return run(cmd)
end

def zipApp(appPath , zipName)
  cmd = "ditto -c -k --sequesterRsrc --keepParent #{appPath} #{zipName}.zip"
  return run(cmd)
end

desc "Clean artifacts"
task :clean do
  run("rm -rf #{zipPath} && rm -rf #{derivedDataPath} && rm xcodebuild.log && rm xcodeipabuild.log")
end

desc "build .app and create .zip artifacts for real iOS Device"
task :build_app => [] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-sdk iphoneos " \
            "-derivedDataPath #{derivedDataPath}"
            "clean build  CODE_SIGN_IDENTITY='iPhone Developer' CODE_SIGNING_REQUIRED=YES"
    build_status = xcodebuild(flags , 'xcodebuild.log')
    exit(-1) unless build_status==0
    build_status = zipApp("#{derivedDataPath}/Build/Products/#{configuration}-iphoneos/#{final_App_Name}.app" , "#{final_App_Name}")
    exit(-1) unless build_status==0
end


desc "build .app and create .zip artifacts for real iOS Device"
task :build_sim_app => [] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-sdk iphonesimulator " \
            "-derivedDataPath #{derivedDataPath}"
            "clean build "
    build_status = xcodebuild(flags , 'xcodebuild.log')
    exit(-1) unless build_status==0
    build_status = zipApp("#{derivedDataPath}/Build/Products/#{configuration}-iphonesimulator/#{final_App_Name}.app" , "#{final_App_Name}_sim")
    exit(-1) unless build_status==0
end

desc "build .app and start agent for real iOS Device"
task :start_Agent => [] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-derivedDataPath #{derivedDataPath} " \
            "-destination 'platform=iOS,id=d0840be174028a4301d250e8a3c70e83d58f74bd' " \
            "clean test "
    build_status = xcodebuild(flags , 'xcodebuild.log')
end

desc "default - build_archive"
task :default => ['clean' , 'build_app' , 'build_sim_app']  

