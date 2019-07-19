source 'https://cdn.cocoapods.org/'
platform :ios, '9.0'
project 'Xcode/Awful'

use_frameworks!
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

install! 'cocoapods', :generate_multiple_pod_projects => true

target 'Awful' do
  pod 'FLAnimatedImage'
  pod 'KVOController'
  pod 'MRProgress/Overlay'
  pod 'Nuke', :git => 'https://github.com/nolanw/Nuke', :branch => 'catalyst-prep'
  pod 'Sourcery'
  pod 'Stencil'

  # Fixes a compile error; I'm happy to pin to some subsequent tagged version if that ever happens.
  pod 'PSMenuItem', :git => 'https://github.com/steipete/PSMenuItem', :commit => '489dbb1c42f8c2c43ac04f0a34faf9aea3b7aa79'

  # Swift 4 support that doesn't crash in KVO. Go back to main pod when it arrives there
  pod 'PullToRefresher', :git => 'https://github.com/MindSea/PullToRefresh', :branch => 'fix-simultaneous-access'

  # Frequently bumps versions without pushing podspec to CocoaPods.
  pod 'SwiftTweaks', :git => 'https://github.com/Khan/SwiftTweaks', :branch => 'master'

  target :AwfulTests
end

target :Smilies do
  pod 'FLAnimatedImage'

  target :SmiliesTests
end

target :SmilieExtractor do
  pod 'FLAnimatedImage'
end

post_install do |installer|
  extension_safe_pods = %w[FLAnimatedImage]
  swift_4_2_pods = %w[PullToRefresher]
  
  installer.pod_target_subprojects.each do |subproj|
    # Replicates `inhibit_all_warnings!`, which was causing problems in Xcode 11 beta 4. If we're past beta 4, maybe reinstate `inhibit_all_warnings!` and see how it goes.
    subproj.build_configurations.each do |config|
      (config.build_settings['OTHER_CLFAGS'] ||= []) << '-w'
      config.build_settings['SWIFT_SUPPRESS_WARNINGS'] = 'YES'
    end

    if extension_safe_pods.include?(subproj.project_name.to_s)
      subproj.build_configurations.each do |config|
        config.build_settings['APPLICATION_EXTENSION_API_ONLY'] = 'YES'
      end
    end
    
    if swift_4_2_pods.include?(subproj.project_name.to_s)
      subproj.build_configurations.each do |config|
        config.build_settings['SWIFT_VERSION'] = '4.2'
      end
    end
  end
end
