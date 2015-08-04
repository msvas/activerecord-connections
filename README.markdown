# ActiveRecord::Connections

ActiveRecord::Connections provides a new way to manage multi-tenant applications based on multiples databases.

## Install

Install the activerecord-connections gem:

    gem install activerecord-connections

Add this line to Gemfile:

    gem 'activerecord-connections'

Runs a bundle install:

    bundle install

## Usage

ActiveRecord::Connections add this syntax to ActiveRecord::Base:

    ActiveRecord::Base.using_connection(connection_name, connection_spec) do
      # Use database connection inside this block
    end

If you are using Rails, you could use this way:

    class ApplicationController < ActionController::Base
      around_filter :handle_customer

      protected

      def handle_customer(&block)
        customer = Customer.find_by_domain!(request.domain)
        customer.using_connection(&block)
      end
    end

    class Customer < ActiveRecord::Base
      serialize :connection_spec

      def using_connection(&block)
        ActiveRecord::Base.using_connection(id, connection_spec, &block)
      end
    end

### Known issues

* Do not manage activerecord migrations for different databases
* Similar objects of different connections do not differ
* Coverage is not the best of the world (you could help easily)
* You will lose the connection if you call using_connection nested

### Sharding and replication

Need sharding or replication? Check out one of these bad boys:

* [ar-octopus](https://github.com/tchandy/octopus)
* [db-charmer](https://github.com/kovyrin/db-charmer)
* [data-fabric](https://github.com/mperham/data_fabric)

## Copyright

Copyright (c) 2011-2012 nohup brasil, released under the MIT license.
