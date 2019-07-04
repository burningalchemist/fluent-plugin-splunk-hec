require 'bundler/gem_tasks'
require 'rake/testtask'

Rake::TestTask.new(:test) do |t|
  t.libs << 'test'
  t.libs << 'lib'
  t.test_files = FileList['test/**/*_test.rb']
  t.verbose = false
  t.warning = false
end

# Building the gem will use the local file mode, so ensure it's world-readable.
# TODO: Fix subfolder permissions
Rake::FixPerms.new(:fix_perms) do
  files = [
    'lib/fluent/plugin/*.rb'
  ].flat_map do |file|
    file.include?('*') ? Dir.glob(file) : [file]
  end

  files.each do |file|
    mode = File.stat(file).mode & 0777
    next unless mode & 0444 != 0444
    puts "Changing mode of #{file} from #{mode.to_s(8)} to "\
         "#{(mode | 0644).to_s(8)}"
    chmod mode | 0444, file
  end
end

task :default => :test
