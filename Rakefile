# Copyright (c) 2021, Gary A. Howard
# BSD-3-Clause
# https://github.com/Traap/docbld/blob/master/LICENSE
# -----------------------------------------------------------------------------
require 'open3'
require 'rake'
require 'rake/clean'

DISTDIR = '_build'

TMP_FILES = ["acn", "acr", "alg", "aux", "bbl", "blg", "dep", "dvi",
             "fdb_latexmk", "fls", "glg", "glo", "gls", "glsdefs", "ist", "log",
             "nav", "out", "out.ps", "src", "snm", "synctex.gz", "toc", "xwm"]

SRC_FILES = Rake::FileList.new("**/*.texx") do |fl|
  TMP_FILES.each do |e|
    CLEAN.include(fl.ext(e))
  end
end

CLOBBER.include(SRC_FILES.ext(".pdf"))

desc "Default deploy task."
task :default => :deploy

desc "Build and deploy documents to #{DISTDIR} directory."
task :deploy => [:remove_distdir, :texx, :copy_files, :clobber] do
  puts ""
  puts "Distribution to #{DISTDIR} has completed."
end

desc "Remove #{DISTDIR} directory."
task :remove_distdir do
  puts "Removing #{DISTDIR}."
  FileUtils.rm_r DISTDIR, :force => true, :verbose => true
  puts ""
end

desc "Compile tex to pdf."
task :texx => SRC_FILES.ext(".pdf")

desc "Copy files to #{DISTDIR}"
task :copy_files do
  puts "\nCopying files to #{DISTDIR}."
  FileUtils.mkdir_p DISTDIR 
  CLOBBER.each do |f|
    begin
      cp f, DISTDIR + "/" + File.basename(f), :verbose => true
    rescue StandardError => e
      puts "ERROR: " + File.basename(f) + " was not copied."
    end
  end
end

desc "List texx files to compile."
task :list_files do
  puts SRC_FILES
end

rule ".pdf" => ".texx" do |t|
  puts "Compiling #{t.source}."
  begin
    command = "latexmk -pdf -synctex=1 -verbose -file-line-error -cd #{t.source}"
    _stdout, stderr, _status = Open3.capture3 command
  rescue StandardError => e
    echo_exception(stderr, e)
  end

  def echo_exception(s, e) 
    puts "Exception Class: #{e.class.name}"
    puts "Exception Message: #{e.class.message}"
    puts "Exception Backtrace: #{e.class.backtrace}"
  end
end
