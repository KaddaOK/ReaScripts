Signal.trap('INT') { abort }

require 'reapack/index'

task :default => :test

task :test do
  require 'minitest'

  class TestMetadata < MiniTest::Test
    def self.test_order
      :alpha
    end
  end

  Dir.glob('**/*.{lua,eel}').each {|file|
    mangled_file = file.downcase
    mangled_file.gsub! /[^\w]+/, '_'

    TestMetadata.send :define_method, "test_#{mangled_file}" do
      errors = ReaPack::Index.validate_file file

      if errors
        flunk errors.join("\n")
      else
        pass
      end
    end
  }

  Minitest.run ARGV
end

task :index do
  ARGV.delete_at 1 if ARGV[1] == '--'

  indexer = ReaPack::Index::Indexer.new Dir.pwd
  indexer.run
end
