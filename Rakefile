# Cloud Foundry Java Buildpack
# Copyright (c) 2013 the original author or authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

RUBY_FILES = Dir.glob(%w(lib/*.rb
                         lib/java_buildpack/*.rb
                         lib/java_buildpack/component/*.rb
                         lib/java_buildpack/container/*.rb
                         lib/java_buildpack/framework/*.rb
                         lib/java_buildpack/framework/spring_auto_reconfiguration/*.rb
                         lib/java_buildpack/jre/*.rb
                         lib/java_buildpack/jre/memory/*.rb
                         lib/java_buildpack/logging/*.rb
                         lib/java_buildpack/repository/*.rb
                         lib/java_buildpack/util/*.rb
                         lib/java_buildpack/util/cache/*.rb
                         lib/java_buildpack/util/play/*.rb)).freeze

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new

require 'yard'
YARD::Rake::YardocTask.new do |t|
  t.files = RUBY_FILES
end

require 'rubocop/rake_task'
Rubocop::RakeTask.new

require 'open3'
task :check_api_doc do
  puts "\nChecking API documentation..."
  output = Open3.capture3("yard stats --list-undoc -- #{RUBY_FILES.join(' ')}")[0]
  if output !~ /100.00% documented/
  	puts "\nFailed due to undocumented public API:\n\n#{output}"
  	exit 1
  else
  	puts "\n#{output}\n"
  end
end

require 'rake/clean'
CLEAN.include %w(.yardoc coverage)
CLOBBER.include %w(doc pkg)

task :default => [ :rubocop, :check_api_doc, :spec ]
