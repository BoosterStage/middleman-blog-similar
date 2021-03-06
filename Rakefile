require 'bundler'
Bundler::GemHelper.install_tasks

require 'cucumber/rake/task'
require 'rspec/core/rake_task'
require 'rubocop/rake_task'
RuboCop::RakeTask.new

RSpec::Core::RakeTask.new(:spec)

Cucumber::Rake::Task.new(:cucumber, 'Run features that should pass') do |t|
  exempt_tags = ''
  exempt_tags << '--tags ~@nojava ' if RUBY_PLATFORM == 'java'
  t.cucumber_opts = "--color --tags ~@wip #{exempt_tags} --strict --format #{ENV['CUCUMBER_FORMAT'] || 'pretty'}"
end

require 'rake/clean'

task test: %w[rubocop spec cucumber]

begin
  require 'cane/rake_task'

  desc 'Run cane to check quality metrics'
  Cane::RakeTask.new(:quality) do |cane|
    cane.no_style = true
    cane.no_doc = true
    cane.abc_glob = 'lib/middleman-blog-similar/**/*.rb'
  end
rescue LoadError
  warn 'cane not available, quality task not provided.'
end

desc 'Build HTML documentation'
task :doc do
  sh 'bundle exec yard'
end
