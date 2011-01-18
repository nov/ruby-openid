require 'rubygems'
require 'rake'

begin
  require 'jeweler'

  class Jeweler
    module Commands
      class ReleaseGemspec
        def run
          unless clean_staging_area?
            system "git status"
            raise "Unclean staging area! Be sure to commit or .gitignore everything first. See `git status` above."
          end
          repo.checkout('releasable')
          regenerate_gemspec!
          commit_gemspec! if gemspec_changed?
          output.puts "Pushing releasable to origin"
          repo.push
        end
      end
    end
  end

  class Jeweler
    module Commands
      class ReleaseToGit
        def run
          unless clean_staging_area?
            system "git status"
            raise "Unclean staging area! Be sure to commit or .gitignore everything first. See `git status` above."
          end
          repo.checkout('releasable')
          repo.push
          if release_not_tagged?
            output.puts "Tagging #{release_tag}"
            repo.add_tag(release_tag)
            output.puts "Pushing #{release_tag} to origin"
            repo.push('origin', release_tag)
          end
        end
      end
    end
  end

  Jeweler::Tasks.new do |gem|
    gem.name = 'nov-ruby-openid'
    gem.authors = ['nov matake']
    gem.email = 'nov@matake.jp'
    gem.homepage = 'http://github.com/nov/ruby-openid/'
    gem.summary = 'A library for consuming and serving OpenID identities.'
    gem.extra_rdoc_files = ["LICENSE", "README", "INSTALL","UPGRADE","CHANGELOG", "CHANGES-2.1.0", "UPGRADE"]
    gem.files.exclude "admin","_darcs","coverage",".gitignore","gemspec"
  end
rescue LoadError
  puts "Jeweler (or a dependency) not available. Install it with: sudo gem install jeweler"
end

require 'rake/testtask'
Rake::TestTask.new(:test) do |test|
  test.libs << 'lib' << 'test'
  test.ruby_opts = ['-rtestutil']
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end

begin
  require 'rcov/rcovtask'
  Rcov::RcovTask.new do |test|
    test.libs << 'test'
    test.ruby_opts = ['-rtestutil']
    test.pattern = 'test/**/test_*.rb'
    test.verbose = true
  end
rescue LoadError
  task :rcov do
    abort "RCov is not available. In order to run rcov, you must: sudo gem install spicycode-rcov"
  end
end

task :test => :check_dependencies

task :default => :test

require 'rake/rdoctask'
Rake::RDocTask.new do |rdoc|
  if File.exist?('VERSION')
    version = File.read('VERSION')
  else
    version = ""
  end

  rdoc.rdoc_dir = 'rdoc'
  rdoc.title = "ruby-openid #{version}"
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('lib/**/*.rb')
end