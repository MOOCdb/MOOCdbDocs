
###############
LabelMe-Text
###############

**************************************
Install Process - Installing Ruby on Rails
**************************************

First, install ruby on your linux environment.  The commands used here come from https://gorails.com/setup/ubuntu/14.10
-In terminal, run these two commands:
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties

-For this project I ran Ruby using RVM, so then run
sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
curl -L https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm
echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
rvm install 2.1.5
rvm use 2.1.5 --default
ruby -v
echo "gem: --no-ri --no-rdoc" > ~/.gemrc

-Install node.js
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs

-Install Rails
gem install rails

-Install depedencies for pg gem **not in online instructions to install rails
sudo apt-get install ruby-dev libpq-dev

Server Setup
-Time to set up the server.  Move to the LabelMe-Text folder and run
bundle install
bundle exec rake db:migrate
bundle exec rake db:reset
bundle exec rails s

-This starts a copy of the server running on the machine’s localhost.  You can access it in your browser at localhost:3000/.

Making an Admin
-Now you want to make an admin user.  Go to the website and sign up with the account info for your admin user.
-From the home page, click on “Create profile”
-Fill in the account details, check the box agreeing not to scrape the data from the website, and click “Create my account”
-Then go to terminal and type Ctrl+C to stop the server from running. Run the following commands:
bundle exec rails console.  
a = User.find_by_email(“your@email.here”)
a.update_column(:admin, true)
exit

Leaving server running
-If you are using a virtual machine and wish the process to keep running after you close the ssh connection, we will use a program called screen to achieve this.

-First we need to install screen.
sudo apt-get install screen

-Start the screen program
screen

-Navigate to the Label-Me Text folder and start the server again
bundle exec rails s

-While the server is running, detach from the current screen using the following command
Ctrl + a, d

-The server should now run even if you stop the ssh session.  To return to the running server terminal, use the command
screen -r

