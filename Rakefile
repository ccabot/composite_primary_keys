require 'rubygems'
require 'rake'
require 'rake/clean'
require 'rake/testtask'
require 'rake/rdoctask'
require 'rake/packagetask'
require 'rake/gempackagetask'

# Set global variable so other tasks can access them
PROJECT_ROOT = File.expand_path(".")
GEM_NAME = 'composite_primary_keys'

# Read the spec file
spec = Gem::Specification.load("#{GEM_NAME}.gemspec")

# Setup Rake tasks for managing the gem
Rake::GemPackageTask.new(spec).define

# Now load in other task files
Dir.glob('tasks/**/*.rake').each do |rake_file|
  load File.join(File.dirname(__FILE__), rake_file)
end

# Set up test tasks for each supported connection adapter
%w(mysql sqlite oracle oracle_enhanced postgresql ibm_db).each do |adapter|
  namespace adapter do
    desc "Run tests using the #{adapter} adapter"
    task "test" do
      ENV["ADAPTER"] = adapter
      Rake::TestTask.new("subtest_#{adapter}") do |t|
        t.libs << "test"
      end
      Rake::Task["subtest_#{adapter}"].invoke
    end
  end
end
