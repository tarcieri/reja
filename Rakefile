# Magical Reia-on-Erjang installer thingy.
# By Tony Arcieri.  License?  Who cares.  I don't claim copyright.

require 'rake/clean'
require 'digest/sha1'

ERJANG_HOME="erjang-0.2"
ERJANG_TARBALL="#{ERJANG_HOME}.tgz"
ERJANG_DISTRIBUTION="http://cloud.github.com/downloads/krestenkrab/erjang/#{ERJANG_TARBALL}"
ERJANG_SHA256="4a28b6953f4ff0259c3e807fb2947a5e8e374580b7aed47ad09e30ab5aa3d05e"

task :default => [:erjang, :reia, :build, :test]

# Helpers

def got_curl?
  !!`curl 2>&1`['curl: try'] # doublebang it!
end

def got_git?
  !!`git`["usage: git"]
end

def download(url, dest)
  raise "You don't have curl. WTF?" unless got_curl?
  system "curl #{url} -o #{dest}"
end

def sha256(path)
  digest = Digest::SHA256.new
  File.open("erjang-0.2.tgz") do |file|
    while data = file.read(4096); digest << data; end
  end
  digest.hexdigest
end

def announce(something)
  puts "\n*** #{something}"
end

# Erjang stuff

task :erjang => [ERJANG_TARBALL, ERJANG_HOME]

file ERJANG_TARBALL do
  announce "Fetching the Erjang distribution"
  download ERJANG_DISTRIBUTION, ERJANG_TARBALL
  
  print "\n*** Checksumming Erjang distribution... "
  if sha256(ERJANG_TARBALL) == ERJANG_SHA256
    puts "ok."
  else
    puts "MISMATCH. Proceed at your own risk!"
  end
end

file ERJANG_HOME => ERJANG_TARBALL do
  announce "Unpacking Erjang"
  system "tar -zxf #{ERJANG_TARBALL}"
  
  announce "Configuring Erjang"
  sh "cd #{ERJANG_HOME} && echo 'y' | ./Install `pwd`"
  puts 'y'
end

# Reia stuff

file :reia do
  raise "No git? How did you even get ahold of this?" unless got_git?
  
  announce "Cloning Reia from Github"
  sh "git clone git://github.com/tarcieri/reia.git"
end

task :build => :reia do
  announce "Ensure Reia is up-to-date..."
  sh "cd reia && git pull"
  
  announce "Building Reia"
  sh "cd reia && rake"
end

task :test do
  announce "Running Reia tests"
  sh "cd reia && ../bin/reia test/runner.re"
end

CLEAN.include ERJANG_TARBALL
CLEAN.include ERJANG_HOME
CLEAN.include 'reia'