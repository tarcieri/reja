# Magical Reia-on-Erjang installer thingy.
# By Tony Arcieri.  License?  Who cares.  I don't claim copyright.

require 'rake/clean'
require 'digest/sha1'

task :default => [:build_erjang, :build_reia, :install_reia, :test]

# Helpers

module Tty extend self
  # The colors, Duke! The colors!l
  def blue; bold 34; end
  def white; bold 39; end
  def red; underline 31; end
  def reset; escape 0; end
  def bold n; escape "1;#{n}" end
  def underline n; escape "4;#{n}" end
  def escape n; "\033[#{n}m" if STDOUT.tty? end
end

def got_git?
  !!`git`["usage: git"] # doublebang it!
end

def clone(repo)
  raise "No git? How did you even get ahold of this?" unless got_git?
  
  name = repo.match(/(\w+)\.git/)[1]
  ohai "Cloning #{name.capitalize} from Github"
  sh "git clone #{repo}"
end

def ensure_up2date(name)
  ohai "Ensuring #{name.capitalize} is up-to-date"
  sh "cd #{name} && git pull"
end

# OHAI!
def ohai(something)
  puts "#{Tty.blue}*** #{Tty.white}#{something}#{Tty.reset}"
end

# Retrieve the ERTS version
def erts_version
  version = `erl -version 2>&1`.strip[/\d+\.\d+\.\d+$/]
  raise "Couldn't determine Erlang version" unless version
  version
end

# Retrieve the Erlang home directory
def erlang_home
  # FIXME: Meh, tried to autodetect this and failed
  #erl_home = `erl -noshell -eval "io:format(code:lib_dir())" -s erlang halt`
  #raise "Couldn't locate erlang_home directory" unless erl_home[/^\//]
  #erl_home
  
  "/usr/local/lib/erlang"
end

# Erjang stuff

file :erjang do
  clone "git://github.com/trifork/erjang.git"
end

task :build_erjang => :erjang do
  ensure_up2date "erjang"
  
  ohai "Building Erjang"
  sh "cd erjang && ant jar"
  ohai "Doctoring erjang/env_cfg to include your Erlang libraries"
  
  env_cfg = "erjang/env_cfg"
  cfg = File.read env_cfg
  cfg.sub!(/ERTS_VSN=.*$/, "ERTS_VSN=#{erts_version}")
  cfg.sub!(/ERL_ROOT=.*$/, "ERL_ROOT=#{erlang_home}")
  File.open(env_cfg, 'w') { |file| file << cfg }
end

# Reia stuff

file :reia do
  clone "git://github.com/tarcieri/reia.git"
end

task :build_reia => :reia do
  ensure_up2date "reia"
  
  ohai "Building Reia"
  sh "cd reia && rake"  
end

task :install_reia do
  ohai "Installing Reia"
  reia_dir = "erjang/lib/reia"
  rm_r reia_dir if File.exists?(reia_dir)
  mkdir_p reia_dir
  cp_r "reia/ebin", reia_dir
end

task :test do
  ohai "Running Reia tests"
  sh "bin/reia test/runner.re"
end

CLEAN.include 'erjang', 'reia'
