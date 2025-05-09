#gem安装环境

    gem environment



找'builder.rb'文件
    如:'/opt/homebrew/lib/ruby/gems/3.4.0/gems/cocoapods-packager-1.5.0/lib/cocoapods-packager/builder.rb'


修改如下
``` ruby
def ios_build_options
    "ARCHS=\'$(ARCHS_STANDARD)\' OTHER_CFLAGS=\'-fembed-bitcode -Qunused-arguments'"
end
```


找'pod_utils.rb'文件

修改如下
``` ruby
unless static_installer.nil?
  static_installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['CLANG_MODULES_AUTOLINK'] = 'NO'
      config.build_settings['GCC_GENERATE_DEBUGGING_SYMBOLS'] = 'NO'
      config.build_settings['EXCLUDED_ARCHS[sdk=iphonesimulator*]'] = 'arm64', 'i386' // <-- inserted here
      config.build_settings['EXCLUDED_ARCHS[sdk=iphoneos*]'] = 'armv7s', 'armv7' // <-- inserted here
    end
  end
  static_installer.pods_project.save
end
```

