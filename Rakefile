# Magical Reia-on-Erjang installer thingy.
# By Tony Arcieri.  License?  Who cares.  I don't claim copyright.

require 'rake/clean'
require 'digest/sha1'

ERJANG_HOME="erjang-0.2"
ERJANG_TARBALL="#{ERJANG_HOME}.tgz"
ERJANG_DISTRIBUTION="http://cloud.github.com/downloads/krestenkrab/erjang/#{ERJANG_TARBALL}"
ERJANG_SHA256="4a28b6953f4ff0259c3e807fb2947a5e8e374580b7aed47ad09e30ab5aa3d05e"

task :default => :erjang

def got_curl?
  !!`curl 2>&1`['curl: try'] # doublebang it!
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

file ERJANG_TARBALL do
  puts "*** Fetching the Erjang distribution"
  download ERJANG_DISTRIBUTION, ERJANG_TARBALL
  
  print "*** Checksumming Erjang distribution... "
  if sha256(ERJANG_TARBALL) == ERJANG_SHA256
    puts "ok."
  else
    puts "MISMATCH. Proceed at your own risk!"
  end
end

task :erjang => ERJANG_TARBALL do
  puts "*** Unpacking Erjang"
  system "tar -zxf #{ERJANG_TARBALL}"
end

CLEAN.include ERJANG_TARBALL
CLEAN.include ERJANG_HOME