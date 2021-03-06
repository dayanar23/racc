# -*- ruby -*-

require 'rubygems'
require 'hoe'

# gem 'rake-compiler', '>= 0.4.1'
# require "rake/extensiontask"

Hoe.plugin :debugging, :doofus, :git, :isolate, :compiler

hoe = Hoe.spec 'racc' do
  developer 'Aaron Patterson', 'aaron@tenderlovemaking.com'
  license "MIT"

  self.extra_rdoc_files  = Dir['*.rdoc']
  self.history_file      = 'ChangeLog'
  self.readme_file       = 'README.rdoc'

  dependency 'rake-compiler', '>= 0.4.1', :developer
  dependency 'minitest',      '~> 4.7',   :developer # stick to stdlib's version

  if RUBY_PLATFORM =~ /java/
    self.spec_extras[:platform]   = 'java'
  else
    self.spec_extras[:extensions] = %w[ext/racc/extconf.rb]
  end
end

task :racc => :isolate

hoe.test_prelude = 'gem "minitest", "~> 4.7"'

file 'lib/racc/parser-text.rb' => ['lib/racc/parser.rb'] do |t|
  source = 'lib/racc/parser.rb'

  open(t.name, 'wb') { |io|
    io.write(<<-eorb)
module Racc
  PARSER_TEXT = <<'__end_of_file__'
#{File.read(source)}
__end_of_file__
end
    eorb
  }
end

task :test => :compile

Hoe.add_include_dirs('.:lib/racc')

task :compile => 'lib/racc/parser-text.rb'
