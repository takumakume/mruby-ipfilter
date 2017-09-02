MRUBY_CONFIG=File.expand_path(ENV["MRUBY_CONFIG"] || "build_config.rb")
MRUBY_VERSION=ENV["MRUBY_VERSION"] || "master"

MAXMINDDB = "mruby/build/mrbgems/mruby-maxminddb/test/fixtures/GeoLite2-City.mmdb"

file :mruby do
  sh "git clone --depth=1 git://github.com/mruby/mruby.git"
  Dir.chdir("./mruby") do
    sh "git checkout #{MRUBY_VERSION} || true"
  end
end

desc "compile binary"
task :compile => :mruby do
  sh "cd mruby && MRUBY_CONFIG=#{MRUBY_CONFIG} rake all"
end

desc "build mruby container for unit test"
task :build do
  sh "docker build -t ipfilter:mruby ."
  sh "docker run -v `pwd`:/tmp -w /tmp -t ipfilter:mruby rake compile"
end

desc "test"
task :test => :setup do
  sh "cd mruby && MRUBY_CONFIG=#{MRUBY_CONFIG} rake all test"
end

desc "cleanup"
task :clean do
  sh "cd mruby && rake deep_clean"
end

desc "setup mmdb"
task :setup => :compile do
  sh "gzip -cd #{MAXMINDDB}.gz > #{MAXMINDDB}" unless File.exist?(MAXMINDDB)
  sh "ln -s $(dirname #{MAXMINDDB}) test/"
end
