require "rbconfig"

BIN = "./workbench/artsy-in-c"
INCLUDE = [RbConfig::CONFIG["rubyhdrdir"], RbConfig::CONFIG["rubyarchhdrdir"]]
LDPATH = [RbConfig::CONFIG["libdir"]]
LINK_LIBS = ["ruby"]
LINK_FRAMEWORKS = ["AudioToolbox", "CoreAudio", "CoreFoundation"]

namespace :dev do
  namespace :setup do
    task :vscode do
      require "json"
      settings = JSON.parse(File.read("./.vscode/c_cpp_properties.json.example"))
      include_path = settings["configurations"][0]["includePath"]
      include_path.concat(INCLUDE)
      include_path.concat(LINK_FRAMEWORKS.map)
      File.write("./.vscode/c_cpp_properties.json", JSON.pretty_generate(settings))
    end
  end

  task :format do
    sh "clang-format -i *.c"
  end
end

directory "workbench"

task :compile => "workbench" do
  include_paths = INCLUDE.map { |i| "-I '#{i}'" }.join(" ")
  lib_paths = LDPATH.map { |ld| "-L '#{ld}'" }.join(" ")
  lib_linkage = LINK_LIBS.map { |l| "-l #{l}" }.join(" ")
  framework_linkage = LINK_FRAMEWORKS.map { |f| "-framework #{f}" }.join(" ")
  sh "clang #{include_paths} #{lib_paths} #{lib_linkage} #{framework_linkage} main.c -o #{BIN}"
end

task :run => :compile do
  sh "bundle exec #{BIN}"
end

task :default => :run