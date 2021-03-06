## Ripple Gateway Appliance Software

The Ripple Gateway appliance is a virtual machine image that
is meant to serve as a standalone package for configuring
and running a Ripple gateway system.

## Installation Instructions

Download the app repo

    git clone git@github.com:stevenzeiler/ripple-gateway-appliance-server.git
		cd ripple-gateway-appliance-server
		npm install

create a postgres user with password

		sudo -u postgres psql
		psql> create database gateway_appliance;
		psql> ALTER USER Postgres WITH PASSWORD '<newpassword>';
		
Next install db-migrate, a tool to define sequel database migration files
and run them up and down in sequence.

    npm install -g db-migrate
    
Edit config/database.json and config/sequelize.js to add new Postgres credentials.

Run database migrations

		sudo -u postgres db-migrate up --config config/database.json --env dev

start the server
	
		node server.js

Once the server is started visit https://0.0.0.0/app to use the app

## Running REST API Integration Tests

    mocha test/integration/

## Creating a User's Gateway Account

In order to interact with a gateway an end user must register
an account, which will include information about their identity.

We will assume the user has a `ripple address` already, and will
be required to to supply login credentials in the form of a `username`
and `password`. The password will be encrypted when stored in the
database along with it salt value.

	The user's record will also contain a reference to a `bank account` (bankAccountId), and a set of `Know Your Customer` (KYC) credentials (kycId).

## Running Database Migrations

The system uses Postgresql for its primary relational datastore, and its 
table schema is managed using the `db-migrate` node.js module: https://github.com/kunklejr/node-db-migrate.

To run the migrations configure database.json with the correct db credentials,
then run `db-migrate up --config ./database.json --env dev`. Db-migrate tracks the migrations that have run in a `migrations` table in the database. To roll back a database migration run `db-migrate down --config ./database.json --env dev`. This migration feature will allow us to alter database tables in the future by pushing changes in git.

## Installing ZeroMQ

		sudo apt-get install libtool autoconf automake uuid-dev build-essential
		cd ~
		wget http://download.zeromq.org/zeromq-4.0.3.tar.gz
		tar zxvf zeromq-4.0.3.tar.gz && cd zeromq-4.0.3
		./configure
		make && make install
		sudo ldconfig
