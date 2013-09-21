##
## CONFIGURATION
##

output_directories = [
  'android',
  'android/res',
  'android/res/drawable-ldpi',
  'android/res/drawable-mdpi',
  'android/res/drawable-hdpi',
  'android/res/drawable-xhdpi',
  'mac',
  'icons',
  'icons/white',
  'icons/black',
  'icons/white/16',
  'icons/white/24',
  'icons/white/32',
  'icons/white/64',
  'icons/black/16',
  'icons/black/24',
  'icons/black/32',
  'icons/black/64',
  'web'
]

android_launcher_target = [
  {:size => 36, :dpi => 120, :dest => 'android/res/drawable-ldpi'},
  {:size => 48, :dpi => 160, :dest => 'android/res/drawable-mdpi'},
  {:size => 72, :dpi => 240, :dest => 'android/res/drawable-hdpi'},
  {:size => 96, :dpi => 320, :dest => 'android/res/drawable-xhdpi'}
]

android_icon_target = [
  {:size => 18, :dpi => 120, :dest => 'android/res/drawable-ldpi'},
  {:size => 24, :dpi => 160, :dest => 'android/res/drawable-mdpi'},
  {:size => 36, :dpi => 240, :dest => 'android/res/drawable-hdpi'},
  {:size => 48, :dpi => 320, :dest => 'android/res/drawable-xhdpi'}
]

white_icon_target = [
  {:size => 16, :dest => 'icons/white/16'},
  {:size => 24, :dest => 'icons/white/24'},
  {:size => 32, :dest => 'icons/white/32'},
  {:size => 64, :dest => 'icons/white/64'}
]

black_icon_target = [
  {:size => 16, :dest => 'icons/black/16'},
  {:size => 24, :dest => 'icons/black/24'},
  {:size => 32, :dest => 'icons/black/32'},
  {:size => 64, :dest => 'icons/black/64'}
]

svg_to_png = [
  # icons
  ['svg/icons/white/*.svg',         white_icon_target],
  ['svg/icons/black/*.svg',         black_icon_target],
  ['svg/kid-jumping.svg',           {:width => 128, :dest => 'icons/leap-small.png'}],
  
  # android
  ['svg/android/icons/*.svg',       android_icon_target],
  ['svg/android/leap-launcher.svg', android_launcher_target],
  ['svg/android/leap-launcher.svg', {:size => 512, :dest => 'android/leap-icon.png'}],
  ['svg/kid-jumping-silhouette-light.svg', android_icon_target],
  ['svg/android/vpn_disconnected.svg', android_icon_target],
  ['svg/android/leap-debug-launcher.svg', android_launcher_target],
  ['svg/android/leap-debug-launcher.svg', {:size => 512, :dest => 'android/leap-debug-icon.png'}],
  ['svg/masks/mask-launcher.svg',   android_launcher_target],
  ['svg/masks/mask-icon.svg',       {:size => 512, :dest => 'android/bitmask-icon.png'}],
      
  # mac
  ['svg/android/leap-launcher.svg', {:size => 128, :dest => 'mac/leap-128x128.png'}],
  ['svg/masks/mask-launcher.svg',   {:size => 128, :dest => 'mac/bitmask-128x128.png'}],

  # web  
  ['svg/kid-jumping-bw.svg',        {:size => 16, :dest => 'web/favicon.png'}],
  ['svg/web/*.svg',                 {:size => 32, :dest => 'web/32'}]
]

png_to_icns = [
  # mac
  [['mac/leap-128x128.png'], 'mac/leap.icns'],
  [['mac/bitmask-128x128.png'], 'mac/bitmask.icns']
]

##
## HELPERS
##

def run(cmd)
  system(cmd + ' >/dev/null 2>/dev/null')
  unless $? == 0
    puts "ERROR: failed to run #{cmd}"
    puts "bailing out."
    exit
  end
end

def render_svg_to_png(source, targets)
  Dir.glob(source).each do |src_file|
    [targets].flatten.each do |target|
      if File.directory?(target[:dest])
        dest_file = File.join(target[:dest], File.basename(src_file).sub(/\.svg$/,'.png'))
      else
        dest_file = target[:dest]
      end
      if !File.exists?(dest_file) || File.mtime(dest_file) < File.mtime(src_file)
        if target[:size]
          height = width = target[:size]
        else
          height = target[:height]
          width = target[:width]
        end
        options = ["--file=#{src_file}", "--export-png=#{dest_file}", "--export-background=0xffffff", "--export-background-opacity=0x00"]
        options << "-w #{width}" if width
        options << "-h #{height}" if height
        options << "--export-dpi=#{target[:dpi]}" if target[:dpi]
        run("inkscape #{options.join ' '}")
        run("optipng #{dest_file}")
        progress
      end
    end
  end
end

def render_png_to_icns(sources, target)
  run("png2icns #{target} #{sources.join(' ')}")
  progress
end

def progress
  putc '.'
  STDOUT.flush
end

##
## RENDER TASK
##

require 'fileutils'

task :default => :render

desc "render SVG images"
task :render do
  Dir.chdir(File.dirname(__FILE__)) do
    output_directories.each do |dir|
      FileUtils.mkdir_p(dir)
    end
    svg_to_png.each do |source, targets|
      render_svg_to_png(source, targets)
    end
    png_to_icns.each do |sources, target|
      render_png_to_icns(sources, target)
    end
  end
  puts
end

desc "clean out rendered images"
task :clean do
  Dir.chdir(File.dirname(__FILE__)) do
    output_directories.each do |dir|
      Dir.entries(dir).grep(/\.(png|icns|jpg)$/).each do |file|
        File.unlink File.join(dir,file)
      end
    end
  end
end

