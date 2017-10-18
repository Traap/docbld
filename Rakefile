# Copyright (c) 2017, Gary A. Howard
# BSD-3-Clause
# https://github.com/Traap/docbld/blob/master/LICENSE
# -----------------------------------------------------------------------------
require 'rake'
require 'rake/clean'

DISTDIR = '_build'

TMP_FILES = ["aux", "bbl", "blg", "dep", "fdb_latexmk", "fls", "glg", "glo",
             "gls", "glsdefs", "ist", "log", "nav", "out", "src", "snm", 
             "synctex.gz", "toc", "xwm"]

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
  puts "Distribution to #{DISTDIR} has completed."
end

desc "Remove #{DISTDIR} directory."
task :remove_distdir do
  puts "\n" "Removing #{DISTDIR}."
  FileUtils.rm_r DISTDIR, :force => true, :verbose => true
end

desc "Compile tex to pdf."
task :texx => SRC_FILES.ext(".pdf")

desc "Copy files to #{DISTDIR}"
task :copy_files do
  puts "\n" "Copying files to #{DISTDIR}."
  FileUtils.mkdir_p DISTDIR 
  CLOBBER.each do |f|
    cp f, DISTDIR + "/" + File.basename(f), :verbose => true
  end
end

desc "List texx files to compile."
task :list_files do
  puts SRC_FILES
end

rule ".pdf" => ".texx" do |t|
  puts "\n" "Compiling #{t.source}."
  sh "latexmk -pdf -quiet -silent -cd #{t.source}"
end
