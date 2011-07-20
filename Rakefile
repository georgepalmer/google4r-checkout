# google4r-checkout Rakefile

require 'rake'
require 'rake/testtask'
require 'rake/gempackagetask'
require 'rake/rdoctask'

desc 'Default: run all tests tests.'
task :default => :'test:all'

desc 'Runs all tests (alias of test:all)'
task :test => :'test:all'

# 
# File sets 
# 
RUBY_FILES  = FileList['lib/**/*.rb', 'lib/**/vendor/**'] 
RDOC_EXTRA  = FileList['README.md','LICENSE', 'CHANGES'] 
EXTRA_FILES = FileList['var/cacert.pem'] 

#
# Documentation
#

desc 'Generate documentation for the google4r library.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'docs'
  rdoc.title    = 'google4r/checkout'
  rdoc.main     = 'README.md'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files = RUBY_FILES + RDOC_EXTRA 
end

#
# Test, test, test! I love saying the word "test"!
#

desc 'Run all tests on the Google4R::Checkout::* classes.'
task :test => ["test:all"]

namespace :test do
  desc 'Run all tests on the Google4R::Checkout::* classes.'
  task :all do
    errors = %w(unit integration system).collect do |task|
      begin
        Rake::Task["test:#{task}"].invoke
        nil
      rescue => e
        task
      end
    end.compact
    abort "Errors running #{errors.join(", ")}!" if errors.any? 
  end  
  
  desc 'Run unit tests on the Google4R::Checkout::* classes.'
  Rake::TestTask.new(:unit) do |t|
    t.libs << 'lib'
    t.test_files = FileList['test/unit/*_test.rb']
    t.verbose = true
  end

  desc 'Run integration tests on the Google4R::Checkout::* classes.'
  Rake::TestTask.new(:integration) do |t|
    t.libs << 'lib'
    t.test_files = FileList['test/integration/*_test.rb']
    t.verbose = true
  end

  desc 'Run system tests on the Google4R::Checkout::* classes.'
  Rake::TestTask.new(:system) do |t|
    t.libs << 'lib'
    t.test_files = FileList['test/system/*_test.rb']
    t.verbose = true
  end
end

#
# Rubygem creation.
#
version = "1.1.beta6"
spec = Gem::Specification.new do |spec|
  spec.platform = Gem::Platform::RUBY

  spec.name = "google4r-checkout"
  spec.summary = "Ruby library to access the Google Checkout service and implement notification handlers."
  spec.description = spec.summary
  spec.version = version
  spec.author = "Tony Chan"
  spec.homepage = "http://code.google.com/p/google-checkout-ruby-sample-code"

  spec.test_files = FileList['test/**/*_test.rb', 'test/test_helper.rb', 'test/frontend_configuration_example.rb'] 
  spec.files      = RUBY_FILES + EXTRA_FILES 
  spec.extra_rdoc_files = RDOC_EXTRA 
  spec.files.reject! { |str| str =~ /^\./ } 
  
  spec.require_path = 'lib'
  spec.required_ruby_version = '>= 1.8.4'
  
  spec.add_dependency('money', '>= 2.3.0')
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.need_tar = true
end
