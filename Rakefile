require 'rake/clean'
require 'rake/testtask'

task :default => [:test]

Rake::TestTask.new(:test) do |t|
  t.test_files = FileList['test/*_test.rb']
  t.ruby_opts = ['-rubygems'] if defined? Gem
end

require 'rubygems'
SPEC = eval(File.read('shotgun.gemspec'))
PACK = "#{SPEC.name}-#{SPEC.version}"

desc 'build packages'
task :package => %W[pkg/#{PACK}.gem pkg/#{PACK}.tar.gz]

directory 'pkg/'

file "pkg/#{PACK}.gem" => %w[pkg/ shotgun.gemspec] + SPEC.files do |f|
  sh "gem build shotgun.gemspec"
  mv File.basename(f.name), f.name
end

file "pkg/#{PACK}.tar.gz" => %w[pkg/] + SPEC.files do |f|
  sh <<-SH
    git archive --prefix=shotgun-#{SPEC.version}/ --format=tar HEAD |
    gzip > '#{f.name}'
  SH
end
