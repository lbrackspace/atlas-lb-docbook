Welcome to Sites v2.

First steps:

1.  For code project (java, ruby), edit the metadata.txt and
    change the TESTPROJECT variable to match the name of the
    hudson job that runs your unit tests.  By convention, that
    be the same name as your git repo.

    RUBY PROJECTS:

    All ruby projects MUST have a Rakefile with "test" and 
    "integration" targets.  In addition, the projects SHOULD
    have ci_reporter support, to get detailed test results.

    If your ruby project outputs a gem (that should be	
    repackaged into native packaging format), make a "gem"
    target in your specfile.  Please do not depend on rubygems,
    and use setup.rb for packaging.  

    Example ruby Rakefile:

    --<CUT>--

    #!/usr/bin/env ruby
    # -*- ruby -*-
    #

    require 'rubygems'
    require 'rake'
    require 'rake/gempackagetask'
    require 'spec/rake/spectask'
    require 'ci/reporter/rake/rspec'

    desc "Run all unit tests"
    Spec::Rake::SpecTask.new('test') do |t|
      t.spec_files = FileList['spec/*spec.rb']
    end

    desc "Run all integration tests"
    Spec::Rake::SpecTask.new('integration') do |t|
      t.spec_files = FileList['spec/*_integration.rb']
    end

    task :test => 'ci:setup:rspec' do
    end

    --<CUT>--

2.  For externally upstreamed package building jobs (where we
    have no corresponding source code), change the BUILDPROJECT
    variable to match the name of the hudson build job.  

    The metadata.txt will be looked for in both the root dir
    and the debian/ dir, so debian packages can (and should)
    move the metadata.txt to there.

    For debian packages, use git-buildpackage and remember
    to create both an upstream branch and a pristine-tar
    branch. 

    Example:

    git push origin origin:refs/heads/upstream
    git push origin origin:refs/heads/pristine-tar
    git checkout --track -b upstream origin/upstream
    git checkout --track -b pristine-tar origin/pristine-tar
    git checkout master
    git-import-orig -u <version> --pristine-tar ../<pkg>_<version>.orig.tar.gz
    git push
    git push --tags

    Now, work and push the package as appropriate.
    


