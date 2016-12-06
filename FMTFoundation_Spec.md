#
# Be sure to run `pod lib lint FMTFoundation.podspec' to ensure this is a
# valid spec before submitting.
#
# Any lines starting with a # are optional, but their use is encouraged
# To learn more about a Podspec see http://guides.cocoapods.org/syntax/podspec.html
#

Pod::Spec.new do |s|
  s.name             = "FMTFoundation"
  s.version          = "0.2.6"
  s.summary          = "基础库"
  s.description      = <<-DESC
运图基础库， 目前包含核心, 网络， 模型等基础模块
  DESC

  s.homepage         = "http://gitlab.private.yaomaiche.app/XXX-wireless/FMTFoundation"
  s.license          = 'MIT'
  s.author           = { "Da" => "da@XXX.com" }
  s.source           = { :git => "http://gitlab.private.yaomaiche.app/XXX-wireless/FMTFoundation.git", :tag => s.version.to_s }

  s.platform     = :ios, '7.0'
  s.requires_arc = true

  s.source_files = 'FMTFoundation/Classes/**/*.{h,m}'
  s.resource = 'FMTFoundation/Assets/**/*.*'

  s.prefix_header_contents = '#import "FMTFoundation.h"'

  s.frameworks = "UIKit",
    "CoreFoundation",
    "CoreText",
    "CoreGraphics",
    "CoreImage",
    "QuartzCore",
    "ImageIO",
    "Accelerate"

  s.libraries = 'z'

  # Core Layer
  # exten objective-c language and iOS SDK

  s.dependency 'libextobjc', '0.4.1'

  s.dependency 'CocoaLumberjack', '3.0.0'
  s.dependency 'Masonry', '1.0.2'

  s.dependency 'GBVersionTracking', '1.3.2'

  # Foundation Layer
  # add basic layer

  s.dependency 'ZipArchive', '1.4.0'

  s.dependency 'FCUUID', '1.3.1'

  s.dependency 'AFNetworking', '3.1.0'

  s.dependency 'Mantle', '2.1.0'
  s.dependency 'ISO8601DateFormatter', '0.8'

  s.dependency 'SAMKeychain', '1.5.2'

  s.dependency 'TPKeyboardAvoiding', '1.3.1'
  s.dependency 'TTTAttributedLabel', '1.13.4'
  s.dependency 'MBProgressHUD', '1.0.0'
  s.dependency 'UINavigationItem+Margin', '2.0.1'
  s.dependency 'DZNEmptyDataSet', '1.8.1'

  s.dependency 'MJRefresh', '3.1.12'
  s.dependency 'SwipeView', '1.3.2'
  s.dependency 'SMPageControl', '1.2'
  s.dependency 'HMSegmentedControl', '1.5.3'
  s.dependency 'FXBlurView', '1.6.4'
  s.dependency 'MJNIndexView', '0.0.1'
  s.dependency 'OAStackView', '1.0.1'

#s.dependency 'Aspects', '1.4.1'
#s.dependency 'PSTAlertController', '1.2.0'

end
