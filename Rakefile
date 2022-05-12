# frozen_string_literal: true

# {{{ Copyright (c) 2021, Gary A. Howard
#     BSD-3-Clause
#     hhttps://github.com/Traap/docbld/blob/master/LICENSE
# ------------------------------------------------------------------------- }}}
# {{{ Requried files.

require 'open3'
require 'rake'
require 'rake/clean'

# ------------------------------------------------------------------------- }}}
# {{{ Echo any exception.

def echo_exception(stderr, exception)
  puts 'Exception'
  puts "  Class: #{exception.class.name}"
  puts "  Message: #{exception.class.message}"
  puts "  Backtrace: #{exception.class.backtrace}"
  puts 'Standard Error'
  puts "  #{stderr}"
end

# ------------------------------------------------------------------------- }}}
# {{{ Run a command and capture output.

def run_command(command)
  _stdout, stderr, _status = Open3.capture3 command
rescue StandardError => e
  echo_exception(stderr, e)
end

# ------------------------------------------------------------------------- }}}
# {{{ Distribution directory.

DISTDIR = '_build'

# ------------------------------------------------------------------------- }}}
# {{{ File extensions created by latexmk and htlatex.

TMP_FILES = [
  '4ct', '4tc', 'acn', 'acr', 'alg', 'aux', 'bbl', 'blg', 'css', 'dep', 'dvi',
  'docx', 'fdb_latexmk', 'fls', 'glg', 'glo', 'gls', 'glsdefs', 'html', 'idv',
  'ist', 'lg', 'log', 'nav', 'out', 'out.ps', 'src', 'snm', 'synctex.gz', 'toc',
  'tmp', 'xref', 'xwm'
].freeze

# ------------------------------------------------------------------------- }}}
# {{{ Files to compile.

SRC_FILES = Rake::FileList.new('**/*.texx') do |fl|
  TMP_FILES.each do |e|
    CLEAN.include(fl.ext(e))
  end
end

# ------------------------------------------------------------------------- }}}
# {{{ Files to distribute.

CLOBBER.include(SRC_FILES.ext('.pdf'))

# ------------------------------------------------------------------------- }}}
# {{{ Task: default

desc 'Default deploy task.'
task default: :deploy

# ------------------------------------------------------------------------- }}}
# {{{ Task: deploy

desc "Build and deploy documents to #{DISTDIR} directory."
task deploy: %i[remove_distdir texx copy_files clobber] do
  puts ''
  puts "Distribution to #{DISTDIR} has completed."
end

# ------------------------------------------------------------------------- }}}
# {{{ Task: remvoe_distdir

desc "Remove #{DISTDIR} directory."
task :remove_distdir do
  puts "Removing #{DISTDIR}."
  FileUtils.rm_r DISTDIR, force: true, verbose: true
  puts ''
end

# ------------------------------------------------------------------------- }}}
# {{{ Task: copy_files

desc "Copy files to #{DISTDIR}"
task :copy_files do
  puts "\nCopying files to #{DISTDIR}."
  FileUtils.mkdir_p DISTDIR
  CLOBBER.each do |f|
    cp f, "#{DISTDIR}/#{File.basename(f)}", verbose: true
  rescue StandardError
    puts "ERROR: #{File.basename(f)} was not copied."
  end
end

# ------------------------------------------------------------------------- }}}
# {{{ Task: list_files

desc 'List texx files to compile.'
task :list_files do
  puts SRC_FILES
end

# ------------------------------------------------------------------------- }}}
# {{{ Task: texx

desc 'Compile tex to pdf.'
task texx: SRC_FILES.ext('.pdf')

rule '.pdf' => '.texx' do |t|
  puts "Compiling #{t.name.ext('.pdf')}."
  run_command "latexmk -pdf -synctex=1 -verbose -file-line-error -cd #{t.source}"
end

# ------------------------------------------------------------------------- }}}
# {{{ Task: docx

desc 'Compile tex to docx.'
task docx: SRC_FILES.ext('.docx')

rule '.docx' => '.texx' do |t|
  puts "Compiling #{t.name.ext('.docx')}."
  run_command "htlatex #{t.source}"
  run_command "pandoc #{t.name.ext('.html')} -o #{t.name.ext('.docx')}"
end

# ------------------------------------------------------------------------- }}}
