require 'pathname'
DATA_DIR = Pathname 'catalog'
WRANGLE_DIR = Pathname 'wrangle'
CORRAL_DIR = WRANGLE_DIR / 'corral'
SCRIPTS = WRANGLE_DIR / 'scripts'
DIRS = {
    'fetched' => CORRAL_DIR / 'fetched',
    'published' => DATA_DIR,
}


F_FILES = Hash[['2010', '2012', '2014', '2016'].map{|y| [y, DIRS['fetched'] / "#{y}.csv"]}]

desc 'Setup the directories'
task :setup do
    DIRS.each_value do |p|
        unless p.exist?
            p.mkpath()
            puts "Created directory: #{p}"
        end
    end
end



task :fetch  => [:setup] do
  F_FILES.each_pair do |year, fname|
    url = "http://www.fec.gov/data/IndependentExpenditure.do?format=csv&election_yr=#{year}"
    sh "curl '#{url}' -o #{fname}"
  end
end

desc "Compile everything"
task :compile  => [:setup] do
  C_FILES.each_value{|fn| Rake::Task[fn].execute() }
end

desc "publish everything"
task :publish  => [:setup] do
  P_FILES.each_value{|fn| Rake::Task[fn].execute() }
end
