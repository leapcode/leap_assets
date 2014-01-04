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
  'web',
  'linux',
  'linux/hicolor',
  'linux/hicolor/24x24',
  'linux/hicolor/24x24/apps',
  'linux/hicolor/32x32',
  'linux/hicolor/32x32/apps',
  'linux/hicolor/48x48',
  'linux/hicolor/48x48/apps',
  'linux/hicolor/64x64',
  'linux/hicolor/64x64/apps',
  'linux/hicolor/128x128',
  'linux/hicolor/128x128/apps',
  'linux/hicolor/256x256',
  'linux/hicolor/256x256/apps',
  'linux/hicolor/scalable',
  'linux/hicolor/scalable/apps'
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

linux_target = [
  {:size => 24, :dest => 'linux/hicolor/24x24/apps/bitmask.png'},
  {:size => 32, :dest => 'linux/hicolor/32x32/apps/bitmask.png'},
  {:size => 48, :dest => 'linux/hicolor/48x48/apps/bitmask.png'},
  {:size => 64, :dest => 'linux/hicolor/64x64/apps/bitmask.png'},
  {:size => 128, :dest => 'linux/hicolor/128x128/apps/bitmask.png'},
  {:size => 256, :dest => 'linux/hicolor/256x256/apps/bitmask.png'}
]

#
# default target filetype is png, unless otherwise specified
#
svg_to_raster = [
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
  ['svg/android/mask-silhouette.svg',	android_icon_target],
      
  # mac
  ['svg/android/leap-launcher.svg', {:size => 128, :dest => 'mac/leap-128x128.png'}],
  ['svg/masks/mask-launcher.svg',   {:size => 128, :dest => 'mac/bitmask-128x128.png'}],
  ['svg/masks/mask-launcher.svg',   {:width => 32, :height => 26, :dest => 'mac/bitmask.tiff'}],

  # web  
  ['svg/kid-jumping-bw.svg',        {:size => 16,  :dest => 'web/favicon.png'}],
  ['svg/web/arrow-down.svg',        {:size => 32,  :dest => 'web/32'}],
  ['svg/masks/mask.svg',            {:width => 128, :dest => 'web/128'}],
  ['svg/web/rainbow-masthead-small.svg', {:dest => 'web/masthead'}],

  # linux
  ['svg/masks/mask-launcher.svg',   linux_target],

  # print
  ['svg/letterhead/letterhead.svg', {:width => 2400, :height => 300, :dest => 'print'}]
]

png_to_icns = [
  # mac
  ['mac/leap-128x128.png', {:dest => 'mac/leap.icns'}],
  ['mac/bitmask-128x128.png', {:dest => 'mac/bitmask.icns'}]
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

def render_svg_to_raster(source, targets)
  render_changed(source, targets) do |src_file, dest_file, target|
    filetype = File.extname(dest_file)
    if target[:size]
      height = width = target[:size]
    else
      height = target[:height]
      width = target[:width]
    end
    if filetype != '.png'
      real_dest_file = dest_file
      dest_file = dest_file.sub(/#{filetype}$/, '-tmp.png')
    end
    options = ["--file=#{src_file}", "--export-png=#{dest_file}", "--export-background=0xffffff", "--export-background-opacity=0x00"]
    options << "-w #{width}" if width
    options << "-h #{height}" if height
    options << "--export-dpi=#{target[:dpi]}" if target[:dpi]
    run("inkscape #{options.join ' '}")
    run("optipng #{dest_file}")
    if filetype != '.png'
      run("gm convert #{dest_file} #{real_dest_file}")
      File.unlink(dest_file)
    end
  end
end

def render_png_to_icns(source, targets)
  render_changed(source, targets) do |src_file, dest_file|
    run("png2icns #{dest_file} #{src_file}")
  end
end

#
# for a source and target(s), yields (src_file, dest_file, info) for each
# source and destination pair that needs rendering.
#
def render_changed(source, targets, &block)
  Dir.glob(source).each do |src_file|
    [targets].flatten.each do |target|
      if File.directory?(target[:dest])
        dest_file = File.join(target[:dest], File.basename(src_file).sub(/\.svg$/,'.png'))
      else
        dest_file = target[:dest]
      end
      if !File.exists?(dest_file) || File.mtime(dest_file) < File.mtime(src_file)
        yield src_file, dest_file, target
        progress
      end
    end
  end
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
    svg_to_raster.each do |source, targets|
      render_svg_to_raster(source, targets)
    end
    png_to_icns.each do |sources, targets|
      render_png_to_icns(sources, targets)
    end
  end
  puts
  puts "Done."
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

