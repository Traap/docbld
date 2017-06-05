# Copyright (c) 2017, Gary A. Howard
# BSD-3-Clause
# https://github.com/Traap/docbld/blob/master/LICENSE
# -----------------------------------------------------------------------------
require 'rake'
require 'rake/clean'

DISTDIR = '_build'

TMP_FILES = ["aux", "bbl", "blg", "dep", "fdb_latexmk", "fls", "glg", "glo",
             "gls", "glsdefs", "ist", "log", "out", "src", "toc","xwm"]

SRC_FILES = Rake::FileList.new("**/*.texx") do |fl|
  TMP_FILES.each do |e|
    CLEAN.include(fl.ext(e))
  end
end

CLOBBER.include(SRC_FILES.ext(".pdf"))

desc "Default deploy task."
task :default => :deploy

desc "Build and deploy documents to #{DISTDIR}"
task :deploy => [:remove_diskdir, :texx, :copy_files, :clobber] do
  puts "Distribution to #{DISTDIR} has completed."
end

desc "Remove ${DISKDIR}"
task :remove_diskdir do
  puts "\n" "Removing ${DISTDIR}."
  FileUtils.rmdir DISTDIR
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
