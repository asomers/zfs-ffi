require 'rubygems/package_task'
require 'rake/testtask'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new
task :default => :spec

task :clean do
  FileUtils.rm_rf 'pkg'
end

gem_spec = Gem::Specification.new do |spec|
  spec.name = 'zfs'
  spec.version = '0.1.0'
  spec.summary = 'ZFS'
  spec.author = 'Will Andrews <will@firepipe.net>'
  spec.has_rdoc = true
  candidates = Dir.glob("{lib}/**/*")
  spec.files = candidates.delete_if {|c| c.match(/\.swp|\.svn|html|pkg/)}
end

topdir = File.dirname(__FILE__)
libdir = "#{topdir}/lib"

namespace :test do
  desc "Perform a basic Ruby compile test"
  task :compile do
    sh "ruby -I#{libdir} -rzfs -e 'puts ZFS.class' >/dev/null"
  end
end

Rake::TestTask.new do |t|
  t.name = "test:functional"
  t.libs << "test"
  t.test_files = FileList['test/functional/**/*_test.rb']
end

# Don't ship any gems without performing some sanity tests.
task :gem => :test
task :gem => :spec

Gem::PackageTask.new(gem_spec) do |pkg|
  pkg.need_tar = false
  pkg.need_zip = false
end
