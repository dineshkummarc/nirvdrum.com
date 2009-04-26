desc "deploy site to nirvdrum.com"
task :deploy do
  require 'rubygems'
  require 'highline/import'
  require 'net/ssh'

  branch = "master"

  username = ask("Username:  ") { |q| q.echo = true }
  password = ask("Password:  ") { |q| q.echo = "*" }

  Net::SSH.start('nirvdrum.com', username, :port => 22, :password => password) do |ssh|
    commands = <<EOF
cd /var/www/nirvdrum.com/cached-copy
git checkout #{branch}
git pull origin #{branch}
git checkout -f
rm -rf _site
jekyll --no-auto
mv _site ../_#{branch}
mv ../#{branch} _old
mv ../_#{branch} ../#{branch}
rm -rf _old
EOF
    commands = commands.gsub(/\n/, "; ")
    ssh.exec commands
  end
end