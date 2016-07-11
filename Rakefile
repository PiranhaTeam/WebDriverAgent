require 'rubygems'  

project_file = "WebDriverAgent.xcodeproj"
scheme_name = "WebDriverAgentRunner"
configuration = "Debug"
derivedDataPath = "./derivedData"
archivePath = "./WebDriverAgentRunner.xcarchive"
ipaPath = "./WebDriverAgentRunner.ipa"
exportProvisioningProfile = "'iOSTeam Provisioning Profile: *'"

def run(command, min_exit_status = 0)
  puts "Executing: `#{command}`"
  system(command)
  return $?.exitstatus
end

def xcodebuild(flags , logfile)
  cmd = "set -o pipefail && xcrun xcodebuild #{flags} | tee #{logfile} | xcpretty"
  return run(cmd)
end
 
desc "Clean ipa artifacts"
task :clean_ipa do
  run("rm -rf #{ipaPath} && rm -rf #{archivePath} && rm -rf #{derivedDataPath} && rm xcodebuild.log && rm xcodeipabuild.log")
end

desc "build xcarchive artifacts for real iOS Device"
task :build_archive => ['clean_ipa'] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-sdk iphoneos " \
            "-derivedDataPath #{derivedDataPath}"
            "-archivePath #{archivePath} " \
            "clean build "
    build_status = xcodebuild(flags , 'xcodebuild.log')
    exit(-1) unless build_status==0
end


desc "build xcarchive artifacts for real iOS Device"
task :build_sim_archive => ['clean_ipa'] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-sdk iphonesimulator " \
            "-derivedDataPath #{derivedDataPath}"
            "-archivePath #{archivePath} " \
            "clean build "
    build_status = xcodebuild(flags , 'xcodebuild.log')
    exit(-1) unless build_status==0
end


# desc "build ipa"
# task :build_ipa =>  ['build_archive'] do
#     flags = "-exportArchive -exportFormat IPA " \
#             "-archivePath #{archivePath} " \
#             "-exportPath #{ipaPath} " \
#             "-exportProvisioningProfile #{exportProvisioningProfile}" 
#     build_status = xcodebuild(flags , 'xcodeipabuild.log')
#     exit(-1) unless build_status==0
# end

desc "default - build_archive"
task :default => ['build_archive']  

