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
  'status/dark/16',
  'status/dark/32',
  'status/dark/64',
  'status/light/16',
  'status/light/32',
  'status/light/64',
  'qt/dark/16',
  'qt/dark/32',
  'qt/dark/64',
  'qt/light/16',
  'qt/light/32',
  'qt/light/64'
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

status_dark_icon_target = [
  {:size => 16, :dest => 'status/dark/16'},
  {:size => 32, :dest => 'status/dark/32'},
  {:size => 64, :dest => 'status/dark/64'}
]

status_light_icon_target = [
  {:size => 16, :dest => 'status/light/16'},
  {:size => 32, :dest => 'status/light/32'},
  {:size => 64, :dest => 'status/light/64'}
]

qt_dark_icon_target = [
  {:size => 16, :dest => 'qt/dark/16'},
  {:size => 32, :dest => 'qt/dark/32'},
  {:size => 64, :dest => 'qt/dark/64'}
]

qt_light_icon_target = [
  {:size => 16, :dest => 'qt/light/16'},
  {:size => 32, :dest => 'qt/light/32'},
  {:size => 64, :dest => 'qt/light/64'}
]

svg_to_png = [
  ['svg/android/leap-launcher.svg', android_launcher_target],
  ['svg/android/leap-launcher.svg', {:size => 512, :dest => 'android/leap-icon.png'}],
  ['svg/android/icons/*.svg',       android_icon_target],
  ['svg/status/dark/*.svg',         status_dark_icon_target],
  ['svg/status/light/*.svg',        status_light_icon_target],
  ['svg/qt/dark/*.svg',             qt_dark_icon_target],
  ['svg/qt/light/*.svg',            qt_light_icon_target],
  ['svg/android/leap-launcher.svg', {:size => 128, :dest => 'mac/leap-128x128.png'}],
  ['svg/kid-jumping.svg', {:width => 128, :dest => 'qt/leap-small.png'}]
]

png_to_icns = [
  [['mac/leap-128x128.png'], 'mac/leap.icns']
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
      dest_file = if File.directory?(target[:dest])
        File.join(target[:dest], File.basename(src_file).sub(/\.svg$/,'.png'))
      else
        target[:dest]
      end
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

desc "render SVG images to PNGs"
task :render do
  Dir.chdir(File.dirname(__FILE__)) do
    output_directories.each do |dir|
      FileUtils.mkdir_p(dir)
      Dir.entries(dir).grep(/\.(png|icns|jpg)$/).each do |file|
        File.unlink File.join(dir,file)
      end
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
