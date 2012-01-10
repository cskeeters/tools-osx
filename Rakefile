
class Dir
  def self.nonhidden_entries dirname
    self.entries(dirname).grep(/^[^.]/)
  end
end

def sudo # http://dpk.org.uk/h
  if ENV["USER"] != "root"
    exec("sudo #{ENV['_']} #{ARGV.join(' ')}")
  end
end

task :install do
  sudo
  
  if File.exist? '/usr/local/bin/macrubyc' # this should probably be the rake default task.
    print 'Precompiling & installing `dict`... '
    Dir.mkdir './.tmp/'
    File.open './.tmp/dict.rb', 'w' do |f|
      f.print File.read('./bin/dict')
    end
    `/usr/local/bin/macrubyc -o dict ./.tmp/dict.rb`
    `rm -rf ./.tmp/`
    `install -b ./dict /usr/local/bin`
    File.delete './dict'
    puts 'Done.'
  else
    print "\nWARNING: You do not appear to have MacRuby, so `dict` will \n"
    puts '         not be installed, but the rest of the tools will be.'
    puts '         If you would like to install `dict`, please install '
    puts '         MacRuby from http://www.macruby.org/ and then '
    print "         re-run `sudo #{$0} #{ARGV.join(' ')}`.\n\n"
  end
    
  Dir.mkdir '/usr/local/bin/' if not File.exist? '/usr/local/bin/'
  bins = Dir.nonhidden_entries('./bin/')
  bins.delete 'dict'
  print 'Installing: ' + bins.map {|x| '`' + x + '`'}.join(', ') + '... '
  system 'install -b '+bins.map {|x| './bin/'+x}.join(' ')+' /usr/local/bin/'
  puts "Done." 
end

task :uninstall do
  sudo
  
  Dir.nonhidden_entries('./bin/').each do |x|
    f = '/usr/local/bin/'+x
    if File.exist? f
      print "Removing `#{x}`... "
      File.delete f
      puts "Done."
    end
  end
end



